## 💡 核心指导思想
> “当你用事件解决了代码耦合的问题时，你同时也面临着失去代码可读性与引发内存泄漏的风险。”
> —— **规范使用的目的：在保持解耦的同时，保证内存安全与可追溯性。**

---

## 🚫 传统事件滥用的“三大死罪”
1. **内存泄漏 (Memory Leak)**：发布者持有订阅者的引用，订阅者被销毁后未注销事件，导致垃圾回收器（GC）无法回收内存。
2. **幽灵调用 (NullReferenceException)**：事件触发时，调用了已经被 Unity 销毁的 GameObject/组件上的方法。
3. **事件面条 (Event Spaghetti)**：事件多重嵌套（A触发B，B触发C），无法通过 `Ctrl+点击` 追踪溯源，Debug 如同走迷宫。

---

## ✅ 规范一：生命周期必须严格成对 (`+=` 与 `-=`)
**铁律：只要有订阅，就必须有注销！**
在 Unity 中，强烈建议摒弃 `Start/OnDestroy`，全面改用 `OnEnable/OnDisable` 来管理事件。

### 👨‍💻 代码模板：
```csharp
public class UIManager : MonoBehaviour
{
    private void OnEnable() 
    {
        // 物体激活时订阅
        PlayerHealth.OnPlayerDied += ShowGameOverUI;
    }

    private void OnDisable() 
    {
        // 物体隐藏或被销毁时必定执行！在此注销！
        PlayerHealth.OnPlayerDied -= ShowGameOverUI;
    }

    private void ShowGameOverUI() 
    {
        gameObject.SetActive(true);
    }
}
```

---

## ✅ 规范二：防御性触发与接收
永远不要假设你的监听者都还活着，或者事件一定有人监听。

### 1. 安全触发 (Safe Invoke)
不要直接写 `OnEvent()`，始终使用语法糖 `?.Invoke()`。
```csharp
// ❌ 错误示范：如果没人监听，直接报错 NullReference
OnPlayerDied(); 

// ✅ 正确示范：如果没人监听，什么都不做
OnPlayerDied?.Invoke(); 
```

### 2. 安全接收 (Safe Receive)
如果是延迟调用的事件，接收端必须验证自身是否存活。
```csharp
private void OnDelayedEventReceived()
{
    // 如果此物体已经被 Destroy，直接拦截（处理 Unity 的假 Null 问题）
    if (this == null || !gameObject.activeInHierarchy) return;
    
    // 执行逻辑...
}
```

---

## ✅ 规范三：严格区分“局部”与“全局”
不要把所有事件都塞进一个巨大的 `GlobalEventManager.cs` 里！

*   **局部事件 (Local Event)**：只关乎物体本身的状态变化（如：怪物的血量变化）。
    *   **做法**：直接写在怪物的脚本上 `public event Action<float> OnHpChanged;`。
*   **全局事件 (Global Event)**：跨越系统的重大事件（如：游戏暂停、关卡加载、玩家死亡）。
    *   **做法**：使用 **ScriptableObject (SO) 事件总线**（见下文）。

---

## 🚀 终极规范：SO 事件总线架构 (数据驱动解耦)
将无形的代码事件，变成 Project 文件夹中有形的 Asset（资产），实现**终极解耦**与**极佳的可追溯性**。

### 核心运作机制：对讲机模式
1. 建立一个频道（SO 资产）。
2. **发布者**只对着频道喊话（不用管谁在听）。
3. **接收者**只监听频道的动静（不用管谁在喊）。

### 必备脚本一：频道 (GameEventSO.cs)
```csharp
using System.Collections.Generic;
using UnityEngine;

[CreateAssetMenu(menuName = "Events/Game Event")]
public class GameEventSO : ScriptableObject
{
    private List<GameEventListener> listeners = new List<GameEventListener>();

    public void Raise()
    {
        // 倒序遍历，防止执行过程中有听众移除自己导致索引越界
        for (int i = listeners.Count - 1; i >= 0; i--)
            listeners[i].OnEventRaised();
    }

    public void RegisterListener(GameEventListener listener)
    {
        if (!listeners.Contains(listener)) listeners.Add(listener);
    }

    public void UnregisterListener(GameEventListener listener)
    {
        if (listeners.Contains(listener)) listeners.Remove(listener);
    }
}
```

### 必备脚本二：收音机 (GameEventListener.cs)
```csharp
using UnityEngine;
using UnityEngine.Events;

public class GameEventListener : MonoBehaviour
{
    public GameEventSO EventToListen;
    public UnityEvent Response; // 让策划在 Inspector 面板中连线

    private void OnEnable()
    {
        if (EventToListen != null) EventToListen.RegisterListener(this);
    }

    private void OnDisable()
    {
        if (EventToListen != null) EventToListen.UnregisterListener(this);
    }

    public void OnEventRaised()
    {
        Response?.Invoke();
    }
}
```
**工作流优势**：通过在资产上右键 -> `Find References In Scene`，一秒钟查出全场有哪些脚本在监听此事件，彻底告别 Bug 迷宫！

---

## 🌳 决策树：我到底该用什么方式通信？

在写下一段通信代码前，按照以下流程拷问自己：

1. **A 控制 B，且 A 是直接发起者吗？** (例如：玩家点击鼠标 -> 枪械开火)
   👉 **不要用事件！** 保持简单，通过接口或直接持有引用调用 `B.Fire()`。
2. **A 发生了变化，只有靠近 A 的物体才关心吗？** (例如：玩家掉血 -> 玩家头顶的血条更新)
   👉 使用 **局部 Action 事件** (`public event Action`)。
3. **A 发生了变化，整个游戏的系统（UI、成就、音效）都想知道吗？** (例如：Boss被击杀)
   👉 使用 **ScriptableObject 事件总线**。