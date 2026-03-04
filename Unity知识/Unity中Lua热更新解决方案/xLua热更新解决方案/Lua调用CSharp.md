### Lua使用C#类

```lua
--CS.命名空间.类名
--Unity的类 比如 GameObject Transform等等 —— CS.UnityEngine.类名
--CS.UnityEngine.GameObject

--通过C#中的类 实例化一个对象 lua中没有new 所以我们直接 类名括号就是实例化对象
--默认调用的 相当于就是无参构造
local obj1 = CS.UnityEngine.GameObject("New GameObject!")

--为了方便使用 并且节约性能 定义全局变量存储 C#中的类
--相当于取了一个别名
GameObject = CS.UnityEngine.GameObject
local obj3 = GameObject("GameObject 2")

--类中的静态对象 可以直接使用.来调用
local obj4 = GameObject.Find("GameObject 2")

--得到对象中的成员变量  直接对象 . 即可
print(obj4.transform.position)
Debug = CS.UnityEngine.Debug
Debug.Log(obj4.transform.position)

Vector3 = CS.UnityEngine.Vector3
--如果使用对象中的 成员方法 ，一定要加:
obj4.transform:Translate(Vector3.right)
Debug.Log(obj4.transform.position)

--自定义类使用方法相同，只是命名空间不同而已
local t = CS.Test()
t:Speak("test说话")

local t2 = CS.MrTang.Test2()
t2:Speak("test2说话")

--继承了Mono的类
--继承了Mono的类 是不能直接new 
local obj5 = GameObject("加脚本测试")
--通过GameObject的 AddComponent添加脚本
--xlua提供了一个重要方法 typeof 可以得到类的Type
--xlua中不支持 无参泛型函数  所以 我们要使用另一个重载
obj5:AddComponent(typeof(CS.LuaCallCSharp))
```

### Lua使用C#枚举

```lua
--枚举的调用规则和类的调用规则是一样的   CS.命名空间.枚举名.枚举成员
--支持取别名 
PrimitiveType = CS.UnityEngine.PrimitiveType
GameObject = CS.UnityEngine.GameObject

local obj = GameObject.CreatePrimitive(PrimitiveType.Cube)

--自定义枚举 使用方法一样 只是注意命名空间即可
E_MyEnum =  CS.E_MyEnum
local c = E_MyEnum.Idle

--枚举转换相关
--数值转枚举
local a = E_MyEnum.__CastFrom(1)
--字符串转枚举
local b = E_MyEnum.__CastFrom("Atk")
```

### Lua使用C#数组、list、Dictionary

```lua
local obj = CS.Lesson3()

--Lua使用C#数组相关知识
--长度 userdata
--C#怎么用 lua就怎么用 不能使用#去获取长度
print(obj.array.Length)

--访问元素
print(obj.array[0])

--lua中索引从1开始，但是遍历是C#的规则，从0开始
for i=0,obj.array.Length-1 do
	print(obj.array[i])
end

--Lua中创建一个C#的数组 Lua中表示数组和List可以用表 
--创建C#中的数组 使用 Array类中的静态方法即可
local array2 = CS.System.Array.CreateInstance(typeof(CS.System.Int32), 10)
print(array2.Length)
print(array2[0])
print(array2)

--Lua调用C# list相关知识点
--调用成员方法用冒号
obj.list:Add(1)
obj.list:Add(2)
obj.list:Add(3)

--长度
print(obj.list.Count)

--遍历
for i=0,obj.list.Count - 1 do
	print(obj.list[i])
end
print(obj.list)

--在Lua中创建一个List对象
--老版本
local list2 = CS.System.Collections.Generic["List`1[System.String]"]()
list2:Add("new member")

--新版本 >v2.1.12
--相当于得到了一个 List<string> 的一个类别名 需要再实例化
local List_String = CS.System.Collections.Generic.List(CS.System.String)
local list3 = List_String()
list3:Add("new member")

--Lua调用C# dictionary相关知识点
--使用和C#一致
obj.dic:Add(1, "value1")
print(obj.dic[1])

--遍历
for k,v in pairs(obj.dic) do
	print(k,v)
end

--在Lua中创建一个字典对象
--相当于得到了一个 Dictionary<string, Vector3> 的一个类别名 需要再实例化
local Dic_String_Vector3 = CS.System.Collections.Generic.Dictionary(CS.System.String, CS.UnityEngine.Vector3)
local dic2 = Dic_String_Vector3()
dic2:Add("vector1", CS.UnityEngine.Vector3.right)
for i,v in pairs(dic2) do
	print(i,v)
end

--在Lua中创建的字典 无法直接通过键中括号得到 结果为nil
print(dic2["vector1"])
print(dic2:TryGetValue("vector1"))

--如果要通过键获取值 要通过get_Item和set_Item方法
print(dic2:get_Item("vector1"))
dic2:set_Item("vector1", nil)
print(dic2:get_Item("vector1"))
```

### Lua使用C#拓展方法

**在Lua中使用的类最好都加上[LuaCallCSharp]，提升性能**

```lua
--Lua调用C# 拓展方法相关知识点

preClass = CS.preClass
--使用静态方法
--CS.命名空间.类名.静态方法名()
preClass.Eat()

--成员方法 实例化出来用
local obj = preClass()
--成员方法用冒号
obj:Speak("hello")

--使用拓展方法和使用成员方法一致
--要调用 C#中某个类的拓展方法 那一定要在拓展方法的静态类前面加上LuaCallCSharp特性
obj:Move()
```

### Lua使用C# ref和out函数

```lua
CallRefOutFunction = CS.CallRefOutFunction
local obj = CallRefOutFunction()

--Lua调用C# ref
--ref参数 会以多返回值的形式返回给lua
--如果函数存在返回值 那么第一个值 就是该返回值
--之后的返回值 就是ref的结果 从左到右一一对应
--ref参数 需要传入一个默认值 占位置
--a 相当于 函数返回值
--b 第一个ref
--c 第二个ref
local a,b,c = obj:RefFun(1, 0, 0, 1)
print(a)
print(b)
print(c)

--Lua调用C# out
--out参数 会以多返回值的形式返回给lua
--如果函数存在返回值 那么第一个值 就是该返回值
--之后的返回值 就是out的结果 从左到右一一对应
--out参数 不需要传占位置的值
local a,b,c = obj:OutFun(20,30)
print(a)
print(b)
print(c)

--混合使用时  综合上面的规则
--ref需占位 out不用传
--第一个是函数的返回值  之后 从左到右依次对应ref或者out
local a,b,c = obj:RefOutFun(20,1)
print(a)
print(b)
print(c)
```

### Lua使用C#重载函数

```lua
--Lua调用C# 重载函数
local obj = CS.Calculator()
--虽然Lua自己不支持写重载函数，但是Lua支持调用C#中的重载函数  
print(obj:Calc())
print(obj:Calc(15, 1))

--Lua虽然支持调用C#重载函数
--但是因为Lua中的数值类型只有Number，对C#中多精度的重载函数支持不好
print(obj:Calc(10))
print(obj:Calc(10.2))

--解决重载函数含糊的问题
--xlua提供了解决方案 反射机制 
--这种方法只做了解 尽量别用
--Type是反射的关键类
--得到指定函数的相关信息
local m1 = typeof(CS.Calculator):GetMethod("Calc", {typeof(CS.System.Int32)})
local m2 = typeof(CS.Calculator):GetMethod("Calc", {typeof(CS.System.Single)})

--通过xlua提供的一个方法 把它转成lua函数来使用
local f1 = xlua.tofunction(m1)
local f2 = xlua.tofunction(m2)

--成员方法 第一个参数传对象
--静态方法 不用传对象
print(f1(obj, 10))
print(f2(obj, 10.2))
```

### Lua使用C#委托和事件

```lua
-- Lua调用C# 委托相关知识点
local obj = CS.CallDelegate()

-- 委托是用来装函数的
-- 使用C#中的委托 就是用来装lua函数的
local fun = function()
    print("Lua函数Func")
end

-- Lua中没有复合运算符 不能+=
-- 如果第一次往委托中加函数 因为是nil 不能直接+
-- 所以第一次 要先赋值

-- 开始加函数
obj.del = fun
-- obj.del = obj.del + fun
obj.del = obj.del + fun

-- 委托执行
obj.del()

-- 开始减函数
obj.del = obj.del - fun
obj.del = obj.del - fun

-- 委托执行
obj.del()

-- 清空
-- 清空所有存储的函数
obj.del = nil
-- 清空过后得先赋值
obj.del = fun

-- 委托执行
obj.del()

-- Lua调用C# 事件相关知识点
local fun2 = function()
    print("Event Function")
end
-- 事件加函数
-- 事件加减函数  和 委托非常不一样
-- lua中使用C#事件 加函数 
-- 有点类似使用成员方 冒号事件名("+", 函数变量)
obj:eventAction("+", fun2)

-- 最好不要添加匿名函数
obj:eventAction("+", function()
    print("事件加的匿名函数")
end)

obj:DoEvent()
-- 事件减函数
obj:eventAction("-", fun2)
obj:DoEvent()

-- 事件清楚
-- 清事件 不能直接设空
-- 在C#中 创建名为ClearEvent的方法 用来清空事件
--[[
public void ClearEvent()
{
    eventAction = null;
}
]]
obj:ClearEvent()
obj:DoEvent()
```

### Lua使用C#二维数组

```lua
--Lua调用C# 二维数组
local obj = CS.DoubleDimensionalArray()

--获取长度
print("行：" .. obj.array:GetLength(0))
print("列：" .. obj.array:GetLength(1))

--获取元素
--不能通过[0,0]或者[0][0]访问元素 会报错
print(obj.array:GetValue(0,0))
print(obj.array:GetValue(1,0))
for i=0,obj.array:GetLength(0)-1 do
	for j=0,obj.array:GetLength(1)-1 do
		print(obj.array:GetValue(i,j))
	end
end
```

### Lua使用C#的null和nil比较

```lua
-- Lua调用C# nil和null比较
-- 往场景对象上添加一个脚本 如果存在就不加 如果不存在再加
GameObject = CS.UnityEngine.GameObject
Rigidbody = CS.UnityEngine.Rigidbody

local obj = GameObject("Cube")
-- 得到身上的刚体组件，如果没有刚体就添加
local rig = obj:GetComponent(typeof(Rigidbody))
print(rig)
-- 判断空
-- nil和null 没法进行==比较

-- 方法一
-- if rig:Equals(nil) then

-- 方法二，创建全局函数IsNull
--[[
	function IsNull(obj)
		if obj == nil or obj:Equals(nil) then
			return true
		end
		return false
	end
	]]

-- 方法三，c#中使用扩展方法
--[[
	[LuaCallCSharp]
	public static class UnityObjectExtension
	{
		public static bool IsNull(this UnityEngine.Object obj)
		{
			return obj == null || obj.Equals(null);
		}
	}
	]]

-- if IsNull(rig) then
if rig:IsNull() then
    rig = obj:AddComponent(typeof(Rigidbody))
end
print(rig)
```

### Lua和系统类及委托相互使用

```lua
GameObject = CS.UnityEngine.GameObject
UI = CS.UnityEngine.UI

local slider = GameObject.Find("Slider")
print(slider)
local sliderScript = slider:GetComponent(typeof(UI.Slider))
print(sliderScript)
sliderScript.onValueChanged:AddListener(function(f)
	print(f)
end)
-- c#中为无法编辑的系统类型或第三方库添加特性
--[[
public static class ExtendList
{
	[CSharpCallLua]
	public static List<Type> CSharpCallLuaList = new List<Type>() { 
		typeof(UnityAction<float>) 
	};
	[LuaCallCSharp]
	public static List<Type> LuaCallCSharpList = new List<Type>() { 
		typeof(UnityEngine.UI.Slider) 
	};
}
]]
```

### Lua使用C#协程

```lua
-- Lua调用C#协程

-- xlua提供的一个工具表
-- 一定是要通过require调用之后 才能用
util = require("xlua.util")

-- C#中协程启动都是通过继承了Mono的类 通过里面的启动函数StartCoroutine
GameObject = CS.UnityEngine.GameObject
WaitForSeconds = CS.UnityEngine.WaitForSeconds

-- 在场景中新建一个空物体，然后挂一个脚本上去，脚本继承mono使用它来开启协程
local obj = GameObject("Coroutine")
local mono = obj:AddComponent(typeof(CS.LuaCoroutineRunner))

-- 希望用来被开启的协程函数 
fun = function()
    local a = 1
    while true do
        -- lua中不能直接使用 C#中的 yield return
        -- 就使用lua中的协程返回
        coroutine.yield(WaitForSeconds(1))
        print(a)
        a = a + 1
        if a > 10 then
            -- 停止协程和C#当中一样
            mono:StopCoroutine(b)
        end
    end
end

-- 不能直接将lua函数传入到开启协程中,必须先调用xlua.util中的cs_generator(lua函数)
b = mono:StartCoroutine(util.cs_generator(fun))
```

### Lua使用C#泛型函数

```lua
-- Lua调用C#泛型函数
local obj = CS.Lesson12()

local child = CS.Lesson12.TestChild()
local father = CS.Lesson12.TestFather()

-- lua中只支持有约束有参数的泛型函数
obj:TestFun1(child, father)
obj:TestFun1(father, child)

-- lua中不支持 没有约束的泛型函数
-- lua中不支持 有约束 但是没有参数的泛型函数
-- lua中不支持 非class的约束


-- 补充知识 让上面 不支持使用的泛型函数 变得能用
-- 得到通用函数  
-- 设置泛型类型再使用
-- xlua.get_generic_method(类, "函数名")
local testFun2 = xlua.get_generic_method(CS.Lesson12, "TestFun2")
local testFun2_R = testFun2(CS.System.Int32)
-- 调用方法的方式
-- 成员方法 第一个参数传调用函数的对象
-- 静态方法 不用传
testFun2_R(obj, 1)

-- 以上方法有一定的使用限制
-- Mono打包 这种方式支持使用
-- il2cpp打包  如果泛型参数是引用类型才可以使用
-- il2cpp打包  如果泛型参数是值类型，除非C#那边已经调用过了 同类型的泛型参数 lua中才能够被使用
```