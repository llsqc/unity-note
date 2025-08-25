### 实现继承MonoBehaviour的单例模式基类的注意事项
**继承MonoBehaviour的脚本不能new**
**继承MonoBehaviour的脚本一定得依附在GameObject上**

### 实现挂载式的单例模式基类
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

/// <summary>
/// 挂载式 继承Mono的单例模式基类
/// </summary>
/// <typeparam name="T"></typeparam>
public class SingletonMono<T>: MonoBehaviour where T:MonoBehaviour
{
    private static T instance;

    public static T Instance
    {
        get
        {
            return instance;
        }
    }

    protected virtual void Awake()
    {
        instance = this as T;
    }
}

```

这种方式不建议大家使用
因为很容易被破坏单例模式的唯一性
1. 挂载多个脚本
2. 切换场景回来时，由于场景放置了挂载脚本的对象，回到该场景时 又会有一个该单例模式对象
3. 还可以通过代码动态的添加多个该脚本 也会破坏唯一性

### 实现自动挂载式的单例模式基类
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;


/// <summary>
/// 自动挂载式的 继承Mono的单例模式基类
/// 推荐使用 
/// 无需手动挂载 无需动态添加 无需关心切场景带来的问题
/// </summary>
/// <typeparam name="T"></typeparam>
public class SingletonAutoMono<T> : MonoBehaviour where T : MonoBehaviour
{
    private static T instance;

    public static T Instance
    {
        get
        {
            if (instance == null)
            {
                //动态创建 动态挂载
                //在场景上创建空物体
                GameObject obj = new GameObject();
                //得到T脚本的类名 为对象改名 这样在编辑器中可以明确的看到该
                //单例模式脚本对象依附的GameObject
                obj.name = typeof(T).ToString();
                //动态挂载对应的 单例模式脚本
                instance = obj.AddComponent<T>();
                //过场景时不移除对象 保证它在整个游戏生命周期中都存在
                DontDestroyOnLoad(obj);
            }
            return instance;
        }
    }
}
```

### 潜在的安全问题
1. 构造函数问题：
 继承MonoBehaviour的函数，不能new，所以不用担心公共构造函数
2. 多线程问题：
 Unity主线程中相关内容，不允许其他线程直接调用，很少有这样的需求，所以也不用太担心
3. 重复挂载问题：
	1. 手动重复挂载
	2. 代码重复添加
 需要人为干涉，定规则，或者通过代码逻辑强制处理

### 重复挂载带来的唯一性问题指什么？
对于继承MonoBehaviour的挂载式的单例模式基类
1. 手动挂载多个相同单例模式脚本
2. 代码动态添加多个相同单例模式脚本

### 解决重复挂载带来的安全问题
对于挂载式的单例模式脚本
1. 同个对象的重复挂载
 为脚本添加特性
```c#
 [DisallowMultipleComponent]
```

2. 修改代码逻辑
 判断如果存在对象，移除脚本

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

/// <summary>
/// 挂载式 继承Mono的单例模式基类
/// </summary>
/// <typeparam name="T"></typeparam>
public class SingletonMono<T>: MonoBehaviour where T:MonoBehaviour
{
    private static T instance;

    public static T Instance
    {
        get
        {
            return instance;
        }
    }

    protected virtual void Awake()
    {
        //已经存在一个对应的单例模式对象了 不需要在有一个了
        if(instance != null)
        {
            Destroy(this);
            return;
        }
        instance = this as T;
        //我们挂载继承该单例模式基类的脚本后 依附的对象过场景时就不会被移除了
        //就可以保证在游戏的整个生命周期中都存在 
        DontDestroyOnLoad(this.gameObject);
    }
}

```

对于自动挂载式的单例模式脚本
 制定使用规则，不允许手动挂载或代码添加

### 解决多线程并发来带的问题
继承MonoBehaviour的单例模式
可加可不加，但是建议不加。
因为Unity中的机制是，Unity主线程中处理的一些对象（如GameObject、Transform等等）
是不允许被其他多线程修改访问的，会直接报错
因此我们一般不会通过多线程去访问继承MonoBehaviour的相关对象
既然如何，就不会发生多线程并发问题