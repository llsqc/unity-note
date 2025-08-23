### 最低支持的C#版本
只要是Unity 5.5及以上的版本就支持C# 4版本

### C# 1~4的功能和语法有哪些？
C# 1 —— 委托、事件
C# 2 —— 泛型、匿名方法、迭代器、可空类型
C# 3 ——
	隐式类型、对象集合初始化、Lambda表达式、匿名类型
	自动实现属性、拓展方法、分部类
	Linq相关的表达式树
C# 4 ——
	泛型的协变和逆变
	命名和可选参数
	动态类型

### 命名和可选参数
有了命名参数，我们将不用匹配参数在所调用方法中的顺序
每个参数可以按照参数名字进行指定
```c#
public void Test(int i, float f, bool b) { }
Test(1, 1.2f, true);
Test(f: 3.3f, i: 5, b: false);
Test(b: false, f: 3.4f, i: 3);
```

命名参数可以配合可选参数使用,让我们做到跳过其中的默认参数直接赋值后面的默认参数
```c#
public void Test2(int i, bool b = true, string s = "123") { }
Test2(1, true, "234");
Test2(1, s: "234");
```
可以让我们更方便的调用函数，少写一些重载函数

### 动态类型
关键词：**dynamic**
作用：通过dynamic类型标识变量的使用和对其成员的引用绕过编译时类型检查改为在运行时解析这些操作。
    在大多数情况下，dynamic类型和object类型行为类似，任何非Null表达式都可以转换为dynamic类型。
    dynamic类型和object类型不同之处在于，
    编译器不会对包含类型 dynamic 的表达式的操作进行解析或类型检查
    编译器将有关该操作信息打包在一起，之后这些信息会用于在运行时评估操作。
    在此过程中，dynamic 类型的变量会编译为 object 类型的变量。
    因此，dynamic 类型只在编译时存在，在运行时则不存在。

注意：
1. 使用dynamic功能 需要将Unity的.Net API兼容级别切换为.Net 4.x
2. IL2CPP 不支持 C# dynamic 关键字。它需要 JIT 编译，而 IL2CPP 无法实现
3. 动态类型是无法自动补全方法的，我们在书写时一定要保证方法的拼写正确性

所以该功能只做了解，不建议使用

```c#
public class Test1{
    public void DynamicTest() { }
}

public class Test2{
    public void DynamicTest() { }
}

dynamic dyn = 1;
object obj = 2;

dyn += 2;

print(obj.GetType());
print(dyn.GetType());
print(dyn);

object t = new Test1();
dynamic tmp = t;
tmp.DynamicTest();
```