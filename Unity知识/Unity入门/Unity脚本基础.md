### 脚本基本规则
![[脚本基本规则.png]]

### 生命周期函数

![[生命周期函数图.bmp]]
![[生命周期函数图.bmp]]


```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson1 : MonoBehaviour
{
    #region 知识点一 了解帧的概念
    //Unity 底层已经帮助我们做好了死循环
    //我们需要学习Unity的生命周期函数
    //利用它做好的规则来执行我们的游戏逻辑就行了
    #endregion

    #region 知识点二 生命周期函数的概念
    //所有继承MonoBehavior的脚本 最终都会挂载到GameObject游戏对象上
    //生命周期函数 就是该脚本对象依附的GameObject对象从出生到消亡整个生命周期中
    //会通过反射自动调用的一些特殊函数

    //Unity帮助我们记录了一个GameObject对象依附了哪些脚本
    //会自动的得到这些对象，通过反射去执行一些固定名字的函数
    #endregion

    #region 知识点三 生命周期函数
    //注意：
    //生命周期函数的访问修饰符一般为private和protected
    //因为不需要再外部自己调用生命周期函数 都是Unity自己帮助我们调用的

    //当对象（自己这个类对象）被创建时 才会调用该生命周期函数
    //类似构造函数的存在 我们可以在一个类对象 该创建 进行一些初始化操作
    protected virtual void Awake()
    {
        //在Unity中打印信息的两种方式
        //1. 没有继承MOnoBehavior类的时候
        //Debug.Log("123");
        //Debug.LogError("出错了！！！！！");
        //Debug.LogWarning("警告！！！");
        //2. 继承了MonoBehavior 有一个线程的方法 可以使用
        print("Awake");
    }

    //对于我们来说 想要当一个对象被激活时 进行一些逻辑处理 就可以写在这个函数
    void OnEnable()
    {
        print("OnEnable");
    }

    //主要作用还是用于初始化信息的 但是它相对Awake来说 要晚一点
    //因为它是在对象 进行第一次帧更新之前才会执行的
    void Start()
    {
        print("Start");
    }
    
    //它主要是用于 进行物理更新 
    //它是每一帧的执行的 但是 这里的帧 和游戏帧 有点不同
    //它的时间间隔 是可以在 project setting中的 Time里去设置的
    void FixedUpdate()
    {
        print("FixedUpdate");
    }

    //主要用于处理游戏核心逻辑更新的函数
    void Update()
    {
        print("Update");   
    }

    //一般这个更新是用来处理 摄像机位置更新相关内容的
    //Update和LateUpdate之间 Unity进了一些处理 处理我们动画相关的更新
    void LateUpdate()
    {
        print("LateUpdate");
    }

    //如果我们希望在一个对象失活时做一些处理 就可以在该函数中写逻辑
    void OnDisable()
    {
        print("OnDisable");    
    }

    void OnDestroy()
    {
        print("OnDestroy");    
    }

    #endregion

    #region 知识点四 生命周期函数 支持继承多态
    #endregion
}

//总结
//这些生命周期函数 如果你不打算在其中写逻辑 那就不要在这些出生命周期函数
```

```txt
我们要知道，虽然建议大家不在继承MonoBehavior的类中写构造函数

但是不意味着我们不能写，当我们在继承MonoBehavior的类中写无参构造函数时，你会发现在编辑模式下或者运行后，只要该脚本挂载在场景中，那么该无参构造函数是会被自动执行的。

因为Unity的工作原理中提到的反射机制，Unity实际上通过反射帮助我们实例化了该脚本对象，既然要实例化那么肯定是需要new的，只不过Unity中不需要我们自己new继承了MonoBehavior的类，只要挂载后Unity帮助我们做了这件事。

那么为什么不建议大家写构造函数呢？

1.Unity的规则就是，继承MonoBehavior的脚本不能new只能挂载

2.生命周期函数的Awake是类似构造函数的存在，当对象出生就会自动调用

3.写构造函数反而在结构上会破坏Unity设计上的规范


总结：

如果继承MonoBehavior的脚本想要进行初始化相关，可以在Awake或者Start中进行，搞清这两个生命周期函数的执行时机，根据需求选择在哪里进行初始化。

切记！！继承MonoBehavior的脚本不要new，不要new，不要new！！
```

```txt
不同对象的生命周期函数是在同一个线程中执行的吗？

答案：Unity中所有对象上挂载的生命周期函数都是在一个主线程中按先后执行的

理解：Unity会主动把场景上的对象，对象上挂载的脚本都统统记录下来，在主线程的死循环中，按顺序按时机的通过反射，执行记录的对象身上挂载的脚本的对应生命周期函数
```

### Inspector窗口可编辑的变量

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public enum E_TestEnum
{
    Normal,
    Player,
    Monster
}

[System.Serializable]
public struct MyStruct
{
    public int age;
    public bool sex;
}

[System.Serializable]
public class MyClass
{
    public int age;
    public bool sex;
}

public class Lesson2 : MonoBehaviour
{
    #region Inspector显示的可编辑内容就是脚本的成员变量

    #endregion

    #region 知识点一 私有和保护无法显示编辑
    private int i1;
    protected string str1;
    #endregion

    #region 知识点二 让私有的和保护的也可以被显示
    //加上强制序列化字段特性
    //[SerializeField]
    //所谓序列化就是把一个对象保存到一个文件或数据库字段中去
    [SerializeField]
    private int privateInt;
    [SerializeField]
    protected string protectedStr;
    #endregion

    #region 知识点三 公共的可以显示编辑
    [HideInInspector]
    public int publicInt = 10;
    public bool publicBool = false;
    #endregion

    #region 知识点四 公共的也不让其显示编辑
    //在变量前加上特性
    //[HideInInspector]
    [HideInInspector]
    public int publicInt2 = 50;
    #endregion

    #region 知识点五 大部分类型都能显示编辑
    public int[] array;
    public List<int> list;
    public E_TestEnum type;
    public GameObject gameObj;

    //字典不能被Inspector窗口显示
    public Dictionary<int, string> dic;
    //自定义类型变量
    public MyStruct myStruct;
    public MyClass myClass;
    #endregion

    #region 知识点六 让自定义类型可以被访问
    //加上序列化特性
    //[System.Serializable]
    //字典怎样都不行
    #endregion

    #region 知识点七 一些辅助特性
    //1.分组说明特性Header
    //为成员分组
    //Header特性
    //[Header("分组说明")]
    [Header("基础属性")]
    public int age;
    public bool sex;
    [Header("战斗属性")]
    public int atk;
    public int def;

    //2.悬停注释Tooltip
    //为变量添加说明
    //[Tooltip("说明内容")]
    [Tooltip("闪避")]
    public int miss;

    //3.间隔特性 Space()
    //让两个字段间出现间隔
    //[Space()]
    [Space()]
    public int crit;

    //4.修饰数值的滑条范围Range
    //[Range(最小值, 最大值)]
    [Range(0,10)]
    public float luck;

    //5.多行显示字符串 默认不写参数显示3行
    //写参数就是对应行
    //[Multiline(4)]
    [Multiline(5)]
    public string tips;

    //6.滚动条显示字符串 
    //默认不写参数就是超过3行显示滚动条
    //[TextArea(3, 4)]
    //最少显示3行，最多4行，超过4行就显示滚动条
    [TextArea(3,4)]
    public string myLife;

    //7.为变量添加快捷方法 ContextMenuItem
    //参数1 显示按钮名
    //参数2 方法名 不能有参数
    //[ContextMenuItem("显示按钮名", "方法名")]
    [ContextMenuItem("重置money", "Test")]
    public int money;
    private void Test()
    {
        money = 99;
    }

    //8.为方法添加特性能够在Inspector中执行
    //[ContextMenu("测试函数")]
    [ContextMenu("测试方法")]
    private void TestFun()
    {
        print("测试方法");
    }
    #endregion

    #region 注意
    //1.Inspector窗口中的变量关联的就是对象的成员变量，运行时改变他们就是在改变成员变量
    public int i = 200;
    //2.拖曳到GameObject对象后 再改变脚本中变量默认值 界面上不会改变
    //3.运行中修改的信息不会保存
    #endregion

    private void Start()
    {
        print(privateInt);
        print(protectedStr);
    }

    private void Update()
    {
        print(i);
    }
}
```

### MonoBehavior中的重要内容

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson3 : MonoBehaviour
{
    public Lesson3 otherLesson3;

    void Start()
    {
        #region 知识点一 重要成员
        //1.获取依附的GameObject
        print(this.gameObject.name);
        //2.获取依附的GameObject的位置信息
        //得到对象位置信息
        print(this.transform.position);//位置
        print(this.transform.eulerAngles);//角度
        print(this.transform.lossyScale);//缩放大小
        //这种写法和上面是一样的效果 都是得到依附的对象的位置信息
        //this.gameObject.transform

        //3.获取脚本是否激活
        this.enabled = false;

        //获取别的脚本对象 依附的gameobject和 transform位置信息
        print(otherLesson3.gameObject.name);
        print(otherLesson3.transform.position);
        #endregion

        #region 知识点二 重要方法
        //得到依附对象上挂载的其它脚本

        //1.得到自己挂载的单个脚本
        //根据脚本名获取
        //获取脚本的方法 如果获取失败 就是没有对应的脚本 会默认返回空
        Lesson3_Test t = this.GetComponent("Lesson3_Test") as Lesson3_Test;
        print(t);
        //根据Type获取
        t = this.GetComponent(typeof(Lesson3_Test)) as Lesson3_Test;
        print(t);
        //根据泛型获取 建议使用泛型获取 因为不用二次转换
        t = this.GetComponent<Lesson3_Test>();
        if( t != null )
        {
            print(t);
        }
        
        //只要你能得到场景中别的对象或者对象依附的脚本
        //那你就可以获取到它的所有信息

        //2.得到自己挂载的多个脚本
        Lesson3[] array = this.GetComponents<Lesson3>();
        print(array.Length);
        List<Lesson3> list = new List<Lesson3>();
        this.GetComponents<Lesson3>(list);
        print(list.Count);

        //3.得到子对象挂载的脚本(它默认也会找自己身上是否挂载该脚本)
        //函数是有一个参数的 默认不传 是false 意思就是 如果子对象失活 是不会去找这个对象上是否有某个脚本的
        //如果传true 即使失活也会找
        //得子对象 挂载脚本 单个
        t = this.GetComponentInChildren<Lesson3_Test>(true);
        print(t);
        //得子对象 挂载脚本 多个
        
        Lesson3_Test[] lts = this.GetComponentsInChildren<Lesson3_Test>(true);
        print(lts.Length);
        
        List<Lesson3_Test> list2 = new List<Lesson3_Test>();
        this.GetComponentsInChildren<Lesson3_Test>(true, list2);
        print(list2.Count);

        //4.得到父对象挂载的脚本(它默认也会找自己身上是否挂载该脚本)
        t = this.GetComponentInParent<Lesson3_Test>();
        print(t);
        lts = this.GetComponentsInParent<Lesson3_Test>();
        print(lts.Length);
        //它也有list的 省略不写了 和上面是一样的套路

        //5.尝试获取脚本
        Lesson3_Test l3t;
        //提供了一个更加安全的 获取单个脚本的方法 如果得到了 会返回true
        //然后在来进行逻辑处理即可
        if(this.TryGetComponent<Lesson3_Test>(out l3t))
        {
            //逻辑处理
        }

        #endregion
    }
}

```