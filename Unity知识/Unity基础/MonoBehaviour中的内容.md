### 延迟函数
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson13 : MonoBehaviour
{
    void Start()
    {
        // 知识点一 什么是延迟函数
        //延迟函数顾名思义就是会延时执行的函数，我们可以自己设定延时要执行的函数和具体延时的时间，是MonoBehaviour基类中实现好的方法

        //知识点二 延迟函数的使用
        
        //1.延迟函数
        //Invoke
        //参数一：函数名 字符串
        //参数二：延迟时间 秒为单位
        Invoke("DelayDoSomething", 1);

        //注意：
        //1-1.延时函数第一个参数传入的是函数名字符串
        //1-2.延时函数没办法传入参数 只有包裹一层
        //1-3.函数名必须是该脚本上申明的函数

        //2.延迟重复执行函数
        //InvokeRepeating
        //参数一：函数名字符串
        //参数二：第一次执行的延迟时间
        //参数三：之后每次执行的间隔时间
        InvokeRepeating("DelayRe", 5, 1);
        //注意：
        //它的注意事项和延时函数一致

        //3.取消延迟函数
        
        //3-1取消该脚本上的所有延时函数执行
        CancelInvoke();

        //3-2指定函数名取消
        //只要取消了指定延迟 不管之前该函数开启了多少次 延迟执行 都会统一取消
        CancelInvoke("DelayDoSomething");

        //4.判断是否有延迟函数
        if( IsInvoking() )
        {
            print("存在延迟函数");
        }
        if( IsInvoking("DelayDoSomething") )
        {
            print("存在延迟函数DelayDoSomething");
        }

        // 知识点三 延迟函数受对象失活销毁影响
        //脚本依附对象失活 或者 脚本自己失活，延迟函数可以继续执行 不会受到影响的。脚本依附对象销毁或者脚本移除，延迟函数无法继续执行

        // 总结
        //继承MonoBehavior的脚本可以使用延时相关函数
        
        //函数相关
        //Invoke 延时函数
        //InvokeRepeating 延时重复函数
        //CancelInvoke 停止所有或者指定延时函数
        //IsInvoking 判断是否有延时函数待执行
        
        //使用相关
        //1.参数都是函数名，无法传参数
        //2.只能执行该脚本中申明的函数
        //3.对象或脚本失活无法停止延时函数执行，只有销毁组件或者对象才会停止或者代码停止
    }

    private void OnEnable()
    {
        //对象激活 的生命周期函数中 开启延迟（重复执行的延迟）
    }

    private void OnDisable()
    {
        //对象失活 的生命周期函数中 停止延迟
    }
}

```

### 协同程序
```c#
using System.Collections;
using System.Collections.Generic;
using System.Threading;
using UnityEngine;

public class Lesson14 : MonoBehaviour
{
    Thread t;
    //申明一个变量作为一个公共内存容器
    Queue<Vector3> queue = new Queue<Vector3>();
    Queue<Vector3> queue2 = new Queue<Vector3>();

    void Start()
    {
        // 知识点一 Unity是否支持多线程？
        
        //首先要明确一点，Unity是支持多线程的，只是新开线程无法访问Unity相关对象的内容
        //注意：Unity中的多线程 要记住关闭

        t = new Thread(Test);
        t.Start();

        // 知识点二 协同程序是什么？
        
        // 协同程序简称协程，它是“假”的多线程，它不是多线程
        // 它的主要作用：把可能会让主线程卡顿的耗时的逻辑分时分步执行
        //主要使用场景：
        //异步加载文件
        //异步下载文件
        //场景异步加载
        //批量创建时防止卡顿

        // 知识点三 协同程序和线程的区别
        
        // 新开一个线程是独立的一个管道，和主线程并行执行
        // 新开一个协程是在原线程之上开启，进行逻辑分时分步执行

        // 知识点四 协程的使用
        
        //继承MonoBehavior的类 都可以开启 协程函数
        //第一步：申明协程函数
        //  协程函数2个关键点
        //  1-1返回值为IEnumerator类型及其子类
        //  1-2函数中通过 yield return 返回值; 进行返回

        //第二步：开启协程函数
        //协程函数 是不能够 直接这样去执行的
        //这样执行没有任何效果
        //MyCoroutine(1, "123");
        
        //常用开启方式
        IEnumerator ie = MyCoroutine(1, "123");
        StartCoroutine(ie);
        
        Coroutine c1 = StartCoroutine( MyCoroutine(1, "123") );
        Coroutine c2 = StartCoroutine( MyCoroutine(1, "123"));
        Coroutine c3 = StartCoroutine( MyCoroutine(1, "123"));

        //第三步：关闭协程
        //关闭所有协程
        StopAllCoroutines();

        //关闭指定协程
        StopCoroutine(c1);

        // 知识点五 yield return 不同内容的含义
        //1.下一帧执行
        yield return null;
        //在Update和LateUpdate之间执行

        //2.等待指定秒后执行
        yield return new WaitForSeconds(1f);
        //在Update和LateUpdate之间执行

        //3.等待下一个固定物理帧更新时执行
        yield return new WaitForFixedUpdate();
        //在FixedUpdate和碰撞检测相关函数之后执行

        //4.等待摄像机和GUI渲染完成后执行
        yield return new WaitForEndOfFrame();
        //在LateUpdate之后的渲染相关处理完毕后之后

        //5.一些特殊类型的对象 比如异步加载相关函数返回的对象
        //异步加载资源 异步加载场景 网络加载时使用
        //一般在Update和LateUpdate之间执行

        //6.跳出协程
        yield break;

        // 知识点六 协程受对象和组件失活销毁的影响
        //协程开启后，组件和物体销毁，协程不执行，物体失活协程不执行，组件失活协程执行

        // 总结
        //1.Unity支持多线程，只是新开线程无法访问主线程中Unity相关内容
        //  一般主要用于进行复杂逻辑运算或者网络消息接收等等
        //  注意：Unity中的多线程一定记住关闭
        //2.协同程序不是多线程，它是将线程中逻辑进行分时执行，避免卡顿
        //3.继承MonoBehavior的类都可以使用协程
        //4.开启协程方法、关闭协程方法
        //5.yield return 返回的内容对于我们的意义
        //6.协程只有当组件单独失活时不受影响，其它情况协程会停止
    }

    void Update()
    {
        if( queue.Count > 0 )
        {
            this.transform.position = queue.Dequeue();
        }
    }

    //关键点一： 协同程序（协程）函数 返回值 必须是 IEnumerator或者继承它的类型 
    IEnumerator MyCoroutine(int i, string str)
    {
        print(i);
        //关键点二： 协程函数当中 必须使用 yield return 进行返回
        yield return null;
        print(str);
        yield return new WaitForSeconds(1f);
        print("2");
        yield return new WaitForFixedUpdate();
        print("3");
        //主要会用来 截图时 会使用
        yield return new WaitForEndOfFrame();
        
        while(true)
        {
            print("5");
            yield return new WaitForSeconds(5f);
        }
    }

    private void Test()
    {
        while(true)
        {
            Thread.Sleep(1000);
            //相当于模拟 复杂算法 算出了一个结果 然后放入公共容器中
            System.Random r = new System.Random();
            queue.Enqueue(new Vector3(r.Next(-10,10), r.Next(-10, 10), r.Next(-10, 10)));
            print("123");
        }
    }

    private void OnDestroy()
    {
        t.Abort();
        t = null;
    }
}

```

### 协同程序原理
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson15 : MonoBehaviour
{

    IEnumerator Test()
    {
        print("第一次执行");
        yield return 1;
        print("第二次执行");
        yield return 2;
    }

    void Start()
    {
        // 知识点一 协程的本质
        
        //协程可以分成两部分
        //1.协程函数本体
        //2.协程调度器

        //协程本体就是一个能够中间暂停返回的函数
        //协程调度器是Unity内部实现的，会在对应的时机帮助我们继续执行协程函数

        //Unity只实现了协程调度部分
        //协程的本体本质上就是一个 C#的迭代器方法

        // 知识点二 协程本体是迭代器方法的体现
        
        //1.协程函数本体
        //如果我们不通过 开启协程方法执行协程 
        //Unity的协程调度器是不会帮助我们管理协程函数的
        IEnumerator ie = Test();

        //但是我们可以自己执行迭代器函数内容
        ie.MoveNext();//会执行函数中内容遇到 yield return为止的逻辑
        print(ie.Current);//得到 yield return 返回的内容

        //2.协程调度器
        //继承MonoBehavior后 开启协程
        //相当于是把一个协程函数（迭代器）放入Unity的协程调度器中帮助我们管理进行执行
        //具体的yield return 后面的规则 也是Unity定义的一些规则

        //总结
        //你可以简化理解迭代器函数
        //C#看到迭代器函数和yield return 语法糖
        //就会把原本是一个的 函数 变成"几部分"
        //我们可以通过迭代器 从上到下遍历这 "几部分"进行执行
        //就达到了将一个函数中的逻辑分时执行的目的

        //而协程调度器就是 利用迭代器函数返回的内容来进行之后的处理
        //比如Unity中的协程调度器
        //根据yield return 返回的内容 决定了下一次在何时继续执行迭代器函数中的"下一部分"

        //理论上来说 我们可以利用迭代器函数的特点 自己实现协程调度器来取代Unity自带的调度器

        //协程的本质就是利用C#的迭代器函数"分步执行"的特点加上协程调度逻辑实现的一套分时执行函数的规则
    }
}

```