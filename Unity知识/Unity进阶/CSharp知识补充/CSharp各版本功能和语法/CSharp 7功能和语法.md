### C#7对应的Unity版本
Unity 2018.3支持C# 7 
Unity 2019.4支持C# 7.3 
7.1, 7.2, 7.3相关内容都是基于 7的一些改进

### C#7的新增功能和语法
1. 字面值改进
2. out 参数相关 和 弃元知识点
3. ref 返回值
4. 本地函数
5. 抛出表达式
6. 元组
7. 模式匹配

### 字面值改进
基本概念：在声明数值变量时，为了方便查看数值
        可以在数值之间插入_作为分隔符
主要作用：方便数值变量的阅读

```c#
int i = 9_9123_1239;
print(i);
int i2 = 0xAB_CD_17;
print(i2);
```

### out变量的快捷使用和弃元
用法：不需要再使用带有out参数的函数之前，声明对应变量
作用：简化代码，提高开发效率

1. 以前使用带out函数的写法
```c#
int a;
int b;
Calc(out a, out b);
```

2. 现在的写法
```c#
Calc(out int x, out int y);
print(x);
print(y);
```

3. 结合var类型更简便(但是这种写法在存在重载时不能正常使用，必须明确调用的是谁)
```c#
Calc(out int a, out var b);
print(a);
print(b);
```

4. 可以使用 _ 弃元符号省略不想使用的参数

```c#
Calc(out int c, out _);
print(c);
```

### ref修饰临时变量和返回值
基本概念：使用ref修饰临时变量和函数返回值，可以让赋值变为引用传递
作用：用于修改数据对象中的某些值类型变量

1. 修饰值类型临时变量
```c#
int testI = 100;
ref int testI2 = ref testI;
testI2 = 900;
print(testI);

TestRef r = new TestRef(5, 5);
ref TestRef r2 = ref r;
r2.atk = 10;
print(r.atk);

public struct TestRef
{
    public int atk;
    public int def;

    public TestRef(int atk, int def)
    {
        this.atk = atk;
        this.def = def;
    }
}
```

2. 获取对象中的参数
```c#
ref int atk = ref r.atk;
atk = 99;
print(r.atk);
```

3. 函数返回值
```c#
int[] numbers = new int[] { 1, 2, 3, 45, 5, 65, 4532, 12 };
ref int number = ref FindNumber(numbers, 5);
number = 98765;
print(numbers[4]);

public ref int FindNumber(int[] numbers, int number)
{
    for (int i = 0; i < numbers.Length; i++)
    {
        if (numbers[i] == number)
            return ref numbers[i];
    }
    return ref numbers[0];
}
```

### 本地函数
基本概念：在函数内部声明一个临时函数
注意：
本地函数只能在声明该函数的函数内部使用
本地函数可以使用声明自己的函数中的变量
作用：方便逻辑的封装
建议：把本地函数写在主要逻辑的后面，方便代码的查看

```c#
print(TestTst(10));

public int TestTst(int i)
{
    bool b = false;
    i += 10;
    Calc();
    print(b);
    return i;

    void Calc()
    {
        i += 10;
        b = true;
    }
}


public void Calc(out int a, out int b)
{
    a = 10;
    b = 20;
}

public void Calc(out float a, out float b)
{
    a = 10;
    b = 20;
}

```

### 知识点一 抛出表达式
**throw** 知识回顾
抛出表达式，就是指抛出一个错误
一般的使用方式 都是 throw后面 new 一个异常类

异常基类：**Exception**

```c#
throw new NullReferenceException("1231231");
```

C#自带异常类：

```txt
常见
IndexOutOfRangeException：当一个数组的下标超出范围时运行时引发。
NullReferenceException：当一个空对象被引用时运行时引发。
ArgumentException：方法的参数是非法的
ArgumentNullException： 一个空参数传递给方法，该方法不能接受该参数
ArgumentOutOfRangeException： 参数值超出范围
SystemException：其他用户可处理的异常的基本类
OutOfMemoryException：内存空间不够
StackOverflowException 堆栈溢出

ArithmeticException：出现算术上溢或者下溢
ArrayTypeMismatchException：试图在数组中存储错误类型的对象
BadImageFormatException：图形的格式错误
DivideByZeroException：除零异常
DllNotFoundException：找不到引用的DLL
FormatException：参数格式错误
InvalidCastException：使用无效的类
InvalidOperationException：方法的调用时间错误
MethodAccessException：试图访问思友或者受保护的方法
MissingMemberException：访问一个无效版本的DLL
NotFiniteNumberException：对象不是一个有效的成员
NotSupportedException：调用的方法在类中没有实现
InvalidOperationException：当对方法的调用对对象的当前状态无效时，由某些方法引发。
```

在C# 7中，可以在更多的表达式中进行错误抛出
好处：更节约代码量

1. 空合并操作符后用throw
```c#
InitInfo("123");

private void InitInfo(string str) => jsonStr = str ?? throw new ArgumentNullException(nameof(str));
```

2. 三目运算符后面用throw
```c#
GetInfo("1,2,3", 4);

private string GetInfo(string str, int index)
{
    string[] strs = str.Split(',');
    return strs.Length > index ? strs[index] : throw new IndexOutOfRangeException();
}
```

3. =>符号后面直接throw
```c#
Action action = () => throw new Exception("错了，不准用这个委托");
action();
```