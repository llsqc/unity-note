### Lua解析器
```c#
//引用命名空间
using XLua;

//Lua解析器 能够让我们在Unity中执行Lua 一般情况下 保持它的唯一性
LuaEnv env = new LuaEnv();

//执行Lua语言
env.DoString("print('你好世界')");

//执行一个Lua脚本
//默认寻找脚本的路径在Resources下，Lua脚本后缀要加一个txt
env.DoString("require('Main')");

//帮助我们清除Lua中我们没有手动释放的对象 垃圾回收
//帧更新中定时执行 或者 切场景时执行
env.Tick();

//销毁Lua解析器 
env.Dispose();
```

### 文件加载重定向
```c#
void Start()
{
    LuaEnv env = new LuaEnv();

    //xlua提供的一个路径重定向的方法，允许我们自定义加载Lua文件的规则
    //当我们执行Lua语言require时，相当于执行一个lua脚本，它就会执行我们自定义传入的这个函数
    env.AddLoader(MyCustomLoader);
    env.DoString("require('Main')");
}

//自动执行
private byte[] MyCustomLoader(ref string filePath)
{
    //通过函数中的逻辑去加载Lua文件 传入的参数是require执行的lua脚本文件名拼接一个Lua文件所在路径
    string path = Application.dataPath + "/Lua/" + filePath + ".lua";
    Debug.Log(path);

    //判断文件是否存在
    if (File.Exists(path))
        return File.ReadAllBytes(path);
    else
        Debug.Log("MyCustomLoader重定向失败，文件名为" + filePath);

    return null;
}
```

### Lua解析器管理器

```c#
using System.Collections;
using System.Collections.Generic;
using System.IO;
using UnityEngine;
using XLua;

/// <summary>
/// Lua管理器
/// </summary>
public class LuaMgr : BaseManager<LuaMgr>
{
    private LuaEnv luaEnv;

    /// <summary>
    /// 得到Lua中的_G
    /// </summary>
    public LuaTable Global
    {
        get
        {
            return luaEnv.Global;
        }
    }

    /// <summary>
    /// 初始化解析器
    /// </summary>
    public void Init()
    {
        if (luaEnv != null)
            return;
        luaEnv = new LuaEnv();
        luaEnv.AddLoader(MyCustomLoader);
        luaEnv.AddLoader(MyCustomABLoader);
    }

    private byte[] MyCustomLoader(ref string filePath)
    {
        string path = Application.dataPath + "/Lua/" + filePath + ".lua";

        if (File.Exists(path))
            return File.ReadAllBytes(path);
        else
            Debug.Log("MyCustomLoader重定向失败，文件名为" + filePath);
        return null;
    }

    private byte[] MyCustomABLoader(ref string filePath)
    {
        TextAsset lua = ABMgr.GetInstance().LoadRes<TextAsset>("lua", filePath + ".lua");
        if (lua != null)
            return lua.bytes;
        else
            Debug.Log("MyCustomABLoader重定向失败，文件名为：" + filePath);

        return null;
    }


    /// <summary>
    /// 传入lua文件名 执行lua脚本
    /// </summary>
    /// <param name="fileName"></param>
    public void DoLuaFile(string fileName)
    {
        string str = string.Format("require('{0}')", fileName);
        DoString(str);
    }

    /// <summary>
    /// 执行Lua语言
    /// </summary>
    /// <param name="str"></param>
    public void DoString(string str)
    {
        if(luaEnv == null)
        {
            Debug.Log("解析器为初始化");
            return;
        }
        luaEnv.DoString(str);
    }

    /// <summary>
    /// 释放lua垃圾
    /// </summary>
    public void Tick()
    {
        if (luaEnv == null)
        {
            Debug.Log("解析器为初始化");
            return;
        }
        luaEnv.Tick();
    }

    /// <summary>
    /// 销毁解析器
    /// </summary>
    public void Dispose()
    {
        if (luaEnv == null)
        {
            Debug.Log("解析器为初始化");
            return;
        }
        luaEnv.Dispose();
        luaEnv = null;
    }
}

```

### 全局变量的获取

```c#
int i = LuaMgr.GetInstance().Global.Get<int>("testNumber");

Debug.Log("testNumber:" + i);

i = 10;

//改值
LuaMgr.GetInstance().Global.Set("testNumber", 55);

//值拷贝 不会影响原来Lua中的值
int i2 = LuaMgr.GetInstance().Global.Get<int>("testNumber");
Debug.Log("testNumber_i2:" + i2);
```

### 全局函数的获取
1. 无参无返回：自定义委托、Action、UnityAction、LuaFunction
2. 有参有返回：自定义委托、Func、LuaFunction
3. 多返回：自定义委托（out、ref）、LuaFunction
4. 变长参数：自定义委托、LuaFunction

```c#
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Events;
using XLua;

//无参无返回值的委托
public delegate void CustomCall();

//有参有返回 的委托
[XLua.CSharpCallLua]
public delegate int CustomCall2(int a);

[XLua.CSharpCallLua]
public delegate int CustomCall3(int a, out int b, out bool c, out string d, out int e);
[XLua.CSharpCallLua]
public delegate int CustomCall4(int a, ref int b, ref bool c, ref string d, ref int e);

[XLua.CSharpCallLua]
public delegate void CustomCall5(string a, params int[] args);//变长参数的类型 是根据实际情况来定的

//无参无返回的获取
//委托
CustomCall call = LuaMgr.GetInstance().Global.Get<CustomCall>("testFun");
call();
//Unity自带委托
UnityAction ua = LuaMgr.GetInstance().Global.Get<UnityAction>("testFun");
ua();
//C#提供的委托
Action ac = LuaMgr.GetInstance().Global.Get<Action>("testFun");
ac();
//Xlua提供的一种 获取函数的方式 少用
LuaFunction lf = LuaMgr.GetInstance().Global.Get<LuaFunction>("testFun");
lf.Call();

//有参有返回
CustomCall2 call2 = LuaMgr.GetInstance().Global.Get<CustomCall2>("testFun2");
Debug.Log("有参有返回：" + call2(10));
//C#自带的泛型委托 方便我们使用
Func<int, int> sFun = LuaMgr.GetInstance().Global.Get<Func<int, int>>("testFun2");
Debug.Log("有参有返回：" + sFun(20));
//Xlua提供的
LuaFunction lf2 = LuaMgr.GetInstance().Global.Get<LuaFunction>("testFun2");
Debug.Log("有参有返回：" + lf2.Call(30)[0]);

//多返回值
//使用 out 和 ref 来接收
CustomCall3 call3 = LuaMgr.GetInstance().Global.Get<CustomCall3>("testFun3");
int b;
bool c;
string d;
int e;
Debug.Log("第一个返回值：" + call3(100, out b, out c, out d, out e));
Debug.Log(b + "_" + c + "_" + d + "_" + e);

CustomCall4 call4 = LuaMgr.GetInstance().Global.Get<CustomCall4>("testFun3");
int b1 = 0;
bool c1 = true;
string d1 = "";
int e1 = 0;
Debug.Log("第一个返回值：" + call4(200, ref b1, ref c1, ref d1, ref e1));
Debug.Log(b1 + "_" + c1 + "_" + d1 + "_" + e1);
//Xlua
LuaFunction lf3 = LuaMgr.GetInstance().Global.Get<LuaFunction>("testFun3");
object[] objs = lf3.Call(1000);
for (int i = 0; i < objs.Length; ++i)
{
    Debug.Log("第" + i + "个返回值是：" + objs[i]);
}

//变长参数
CustomCall5 call5 = LuaMgr.GetInstance().Global.Get<CustomCall5>("testFun4");
call5("123", 1, 2, 3, 4, 5, 566, 7, 7, 8, 9, 99);

LuaFunction lf4 = LuaMgr.GetInstance().Global.Get<LuaFunction>("testFun4");
lf4.Call("456", 6, 7, 8, 99, 1);
```

### List和Dictionary映射table

都是浅拷贝

```c#
LuaMgr.GetInstance().Init();
LuaMgr.GetInstance().DoLuaFile("Main");

//同一类型List
List<int> list = LuaMgr.GetInstance().Global.Get<List<int>>("testList");
for (int i = 0; i < list.Count; ++i)
    Debug.Log(list[i]);

//不指定类型 object
List<object> list3 = LuaMgr.GetInstance().Global.Get<List<object>>("testList2");
for (int i = 0; i < list3.Count; ++i)
    Debug.Log(list3[i]);

Dictionary<string, int> dic = LuaMgr.GetInstance().Global.Get<Dictionary<string, int>>("testDic");
foreach (string item in dic.Keys)
    Debug.Log(item + "_" + dic[item]);

Dictionary<object, object> dic3 = LuaMgr.GetInstance().Global.Get<Dictionary<object, object>>("testDic2");
foreach (object item in dic3.Keys)
    Debug.Log(item + "_" + dic3[item]);
```

### 类映射table

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Events;
using XLua;

public class CallLuaClass
{
    //在这个类中去声明成员变量，名字要和Lua的一样
    //公共 私有和保护 没办法赋值
    //如果变量比 lua中的少 就会忽略它，如果变量比 lua中的多 不会赋值 也会忽略
    public int testInt;
    public bool testBool;
    public float testString;
    public UnityAction testFun;
    public CallLuaInClass testInClass;
    public int i;
    public void Test()
    {
        Debug.Log(testInt);
    }
}

public class CallLuaInClass
{
    public int testInInt;
}

public class Lesson7_CallClass : MonoBehaviour
{
    void Start()
    {
        LuaMgr.GetInstance().Init();
        LuaMgr.GetInstance().DoLuaFile("Main");
        //值拷贝 改变了它 不会改变Lua表里的内容
        CallLuaClass obj = LuaMgr.GetInstance().Global.Get<CallLuaClass>("testClas");
        Debug.Log(obj.testString);
        Debug.Log("嵌套：" + obj.testInClass.testInInt);
    }
}
```

### 接口映射table

接口拷贝是**引用拷贝**

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Events;
using XLua;

//接口中是不允许有成员变量的 
//我们用属性来接受
//接口和类规则一样 其中的属性多了少了 不影响结果
[CSharpCallLua]
public interface ICSharpCallInterface
{
    int testInt { get; set; }
    bool testBool { get; set; }
    string testString { get; set; }
    UnityAction testFun { get; set; }
    float testFloat222 { get; set; }
}

LuaMgr.GetInstance().Init();
LuaMgr.GetInstance().DoLuaFile("Main");

ICSharpCallInterface obj = LuaMgr.GetInstance().Global.Get<ICSharpCallInterface>("testClas");
Debug.Log(obj.testInt);

//接口拷贝 是引用拷贝 改了值 lua表中的值也变了
obj.testInt = 10000;
ICSharpCallInterface obj2 = LuaMgr.GetInstance().Global.Get<ICSharpCallInterface>("testClas");
Debug.Log(obj2.testInt);
```

### LuaTable映射table

```c#
using UnityEngine;
using XLua;


LuaMgr.GetInstance().Init();
LuaMgr.GetInstance().DoLuaFile("Main");

//不建议使用LuaTable和LuaFunction 效率低
//引用对象
LuaTable table = LuaMgr.GetInstance().Global.Get<LuaTable>("testClas");
Debug.Log(table.Get<int>("testInt"));
Debug.Log(table.Get<bool>("testBool"));

table.Get<LuaFunction>("testFun").Call();

//改  引用
table.Set("testInt", 55);
Debug.Log(table.Get<int>("testInt"));
LuaTable table2 = LuaMgr.GetInstance().Global.Get<LuaTable>("testClas");
Debug.Log(table2.Get<int>("testInt"));

table.Dispose();
table2.Dispose();
```

**CSharpCallLua特性在自定义委托和接口中使用**