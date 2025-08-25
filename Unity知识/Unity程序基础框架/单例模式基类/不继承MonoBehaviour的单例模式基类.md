### 实现不继承MonoBehaviour的单例模式基类
```c#
public class BaseManager<T> where T:class,new()
{
    private static T instance;

    //属性的方式
    public static T Instance
    {
        get
        {
            if (instance == null)
                instance = new T();
            return instance;
        }
    }

    //方法的方式
    //public static T GetInstance()
    //{
    //    if (instance == null)
    //        instance = new T();
    //    return instance;
    //}
}

```

### 潜在的安全问题
1. 构造函数问题：构造函数可在外部调用 可能会破坏唯一性
2. 多线程问题：当多个线程同时访问管理器时，可能会出现共享资源的安全访问问题

### 构造函数带来的唯一性问题
对于不继承MonoBehaviour的单例模式基类，我们要避免在外部 new 单例模式类对象

### 解决构造函数带来的安全问题
1. 父类变为抽象类
2. 规定继承单例模式基类的类必须显示实现私有无参构造函数
3. 在基类中通过反射来调用私有构造函数实例化对象
 主要知识点：
 利用**Type**中的 **GetConstructor**(约束条件, 绑定对象, 参数类型, 参数修饰符)方法
 来获取私有无参构造函数
```c#
 ConstructorInfo constructor = typeof(T).GetConstructor(
 BindingFlags.Instance | BindingFlags.NonPublic, //表示成员私有方法
   null,                                         //表示没有绑定对象
   Type.EmptyTypes,                              //表示没有参数
   null);                                        //表示没有参数修饰符
```

```c#
using System;
using System.Collections;
using System.Collections.Generic;
using System.Reflection;
using UnityEngine;

/// <summary>
/// 单例模式基类 主要目的是避免代码的冗余 方便我们实现单例模式的类
/// </summary>
/// <typeparam name="T"></typeparam>
public abstract class BaseManager<T> where T : class //,new()
{
    private static T _instance;
    
    public static T Instance
    {
        get
        {
            if (_instance == null)
            {
                Type type = typeof(T);
                ConstructorInfo info = type.GetConstructor(
                    BindingFlags.Instance | BindingFlags.NonPublic,
                    null,
                    Type.EmptyTypes,
                    null);
                if (info != null)
                    _instance = info.Invoke(null) as T;
                else
                    Debug.LogError("没有得到对应的无参构造函数");
            }
            return _instance;
        }
    }
}
```

### 解决多线程并发来带的问题
 建议加锁，避免以后使用多线程时出现并发问题
 比如在处理网络通讯模块、复杂算法模块时，经常会进行多线程并发处理
```c#
using System;  
using System.Collections;  
using System.Collections.Generic;  
using System.Reflection;  
using UnityEngine;  
  
/// <summary>  
/// 单例模式基类 主要目的是避免代码的冗余 方便我们实现单例模式的类  
/// </summary>  
/// <typeparam name="T"></typeparam>  
public abstract class BaseManager<T> where T : class  
{  
    private static T _instance;  
  
    //用于加锁的对象  
    protected static readonly object lockObj = new object();  
  
    //属性的方式  
    public static T Instance  
    {  
        get  
        {  
            if (_instance != null) return _instance;  
            lock (lockObj)  
            {                if (_instance != null) return _instance;  
                Type type = typeof(T);  
                ConstructorInfo info = type.GetConstructor(BindingFlags.Instance | BindingFlags.NonPublic,  
                    null,  
                    Type.EmptyTypes,  
                    null);  
                if (info != null)  
                    _instance = info.Invoke(null) as T;  
                else  
                    Debug.LogError("没有得到对应的无参构造函数");  
            }  
            return _instance;  
        }    
    }
}
```