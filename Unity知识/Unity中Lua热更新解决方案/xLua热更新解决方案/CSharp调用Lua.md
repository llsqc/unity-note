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