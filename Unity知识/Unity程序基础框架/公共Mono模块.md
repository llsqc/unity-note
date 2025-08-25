### 作用
让不继承MonoBehaviour的脚本也能
1. 利用帧更新或定时更新处理逻辑
2. 利用协同程序处理逻辑
3. 进行集中化管理，减少Unity中多脚本中帧更新或定时更新函数的数量

### 原理
实现一个 继承 继承MonoBehaviour单例模式基类 的公共Mono管理器脚本
在其中
1. 通过事件或委托 管理 不继承MonoBehaviour脚本的相关更新函数
2. 提供协同程序开启或关闭的方法
从而达到我们的目的
让不继承MonoBehaviour的脚本也能
3. 利用帧更新或定时更新处理逻辑
4. 利用协同程序处理逻辑
5. 可以统一执行管理帧更新或定时更新相关逻辑

### 实现公共Mono模块
1. 创建MonoMgr继承 自动挂载式的继承MonoBehaviour的单例模式基类
2. 实现Update、FixedUpdate、LateUpdate生命周期函数
3. 声明对应事件或委托用于存储外部函数，并提供添加移除方法，从而达到让不继承MonoBehaviour的脚本可以执行帧更新或定时更新的目的
4. 声明协同程序开启关闭函数，从而达到让不继承MonoBehaviour的脚本可以执行协同程序的目的

```c#
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Events;

public class MonoManager : SingletonAutoMono<MonoManager>
{
    private event UnityAction UpdateEvent;
    private event UnityAction FixedUpdateEvent;
    private event UnityAction LateUpdateEvent;

    /// <summary>
    /// 添加Update帧更新监听函数
    /// </summary>
    /// <param name="action"></param>
    public void AddUpdateListener(UnityAction action)
    {
        UpdateEvent += action;
    }

    /// <summary>
    /// 移除Update帧更新监听函数
    /// </summary>
    /// <param name="action"></param>
    public void RemoveUpdateListener(UnityAction action)
    {
        UpdateEvent -= action;
    }

    /// <summary>
    /// 添加FixUpdate帧更新监听函数
    /// </summary>
    /// <param name="action"></param>
    public void AddFixUpdateListener(UnityAction action)
    {
        FixedUpdateEvent += action;
    }

    /// <summary>
    /// 移除FixUpdate帧更新监听函数
    /// </summary>
    /// <param name="action"></param>
    public void RemoveFixUpdateListener(UnityAction action)
    {
        FixedUpdateEvent -= action;
    }

    /// <summary>
    /// 添加LateUpdate帧更新监听函数
    /// </summary>
    /// <param name="action"></param>
    public void AddLateUpdateListener(UnityAction action)
    {
        LateUpdateEvent += action;
    }

    /// <summary>
    /// 移除LateUpdate帧更新监听函数
    /// </summary>
    /// <param name="action"></param>
    public void RemoveLateUpdateListener(UnityAction action)
    {
        LateUpdateEvent -= action;
    }

    private void Update()
    {
        UpdateEvent?.Invoke();
    }

    private void FixedUpdate()
    {
        FixedUpdateEvent?.Invoke();
    }

    private void LateUpdate()
    {
        LateUpdateEvent?.Invoke();
    }
}
```