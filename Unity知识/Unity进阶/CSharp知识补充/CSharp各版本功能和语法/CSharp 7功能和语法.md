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

public void Calc(out int a, out int b)
{
    a = 10;
    b = 20;
}
```

2. 现在的写法
```c#
Calc(out int x, out int y);
```

3. 结合var类型更简便(但是这种写法在存在重载时不能正常使用，必须明确调用的是谁)
```c#
Calc(out int a, out var b);
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
```

### 抛出表达式
**throw** 知识回顾
抛出表达式，就是指抛出一个错误
一般的使用方式 都是 throw后面 new 一个异常类

异常基类：**Exception**

```c#
throw new System.Exception("出错了");
throw new NullReferenceException("出错了");
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
private void InitInfo(string str) => jsonStr = str ?? throw new ArgumentNullException(nameof(str));
```

2. 三目运算符后面用throw
```c#
private string GetInfo(string str, int index)
{
    string[] strs = str.Split(',');
    return strs.Length > index ? strs[index] : throw new IndexOutOfRangeException();
}
```

3. =>符号后面直接throw
```c#
Action action = () => throw new Exception("委托非法");
action();
```

### 元组
基本概念：多个值的集合，相当于是一种快速构建数据结构类的方式
        一般在函数存在多返回值时可以使用元组 (返回值1类型,返回值2类型,....) 来声明返回值
        在函数内部返回具体内容时通过 (返回值1,返回值2,....)  进行返回
主要作用：提升开发效率，更方便的处理多返回值等需要用到多个值时的需求

1. 无变量名元组的声明(获取值：Item'N'作为从左到右依次的参数，N从1开始)
```c#
(int, float, bool, string) tuple = (1, 5.5f, true, "123");
print(tuple.Item3);
```

2. 有变量名元组的声明
```c#
(int i, float f, bool b, string str) tuple2 = (1, 5.5f, true, "123");
print(tuple2.str);
```

3. 元组可以进行等于和不等于的判断
 数量相同才比较，类型相同才比较，每一个参数的比较是通过 == 比较 如果都是true 则认为两个元组相等
```c#
if (tuple == tuple2)
    print("相等");
else
    print("不相等"); 
```
元组不仅可以作为临时变量 成员变量也是可以的
```c#
public (int, float) tuple;
print(this.tuple.Item1);
```

#### 元组的应用——函数返回值
```c#
private (string str, int i, float f) GetInfo()
{
    return ("123", 2, 5.5f);
}

//无变量名函数返回值
var info = GetInfo();
print(info.Item1);

//有变量名
print(info.f);
```

元组的解构赋值
相当于把多返回值元组拆分到不同的变量中
```c#
int myInt;
string myStr;
float myFloat;
(myStr, myInt, myFloat) = GetInfo();
//(string myStr, int myInt, float myFloat) = GetInfo();
```

丢弃参数
利用传入 下划线_ 达到丢弃该参数不使用的作用
```c#
(string ss, _, _) = GetInfo();
print(ss);
```

#### 元组的应用——字典
字典中的键 需要用多个变量来控制
```c#
Dictionary<(int i, float f), string> dic = new Dictionary<(int i, float f), string>();
dic.Add((1, 2.5f), "123");

if (dic.ContainsKey((1, 2.5f)))
{
    print("存在相同的键");
    print(dic[(1, 2.5f)]);
}
```

### 模式匹配
基本概念：模式匹配时一种语法元素，可以测试一个值是否满足某种条件，并可以从值中提取信息
        在C#7中，模式匹配增强了两个现有的语言结构
        1. is表达式，is表达式可以在右侧写一个模式语法，而不仅仅是一个类型
        2. switch语句中的case
主要作用：节约代码量，提高编程效率

1. 常量模式(is 常量)：用于判断输入值是否等于某个值
```c#
object o = 1.5f;
if (o is 1)
{
    print("o是1");
}
if (o is null)
{
    print("o是null");
}
```

2. 类型模式(is 类型 变量名、case 类型 变量名)：用于判断输入值类型，如果类型相同，将输入值提取出来
判断某一个变量是否是某一个类型，如果满足会将该变量存入你申明的变量中

```c#
//以前的写法
if (o is int)
{
   int i = (int)o;
   print(i);
}

if (o is int i)
{
    print(i);
}

switch (o)
{
    case int value:
        print("int:" + value);
        break;
    case float value:
        print("float:" + value);
        break;
    case null:
        print("null");
        break;
    default:
        break;
}
```

3. var模式：用于将输入值放入与输入值相同类型的新变量中
         相当于是将变量装入一个和自己类型一样的变量中
```c#
if (o is var v)
{
    print(o);
}
```