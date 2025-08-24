### C#8对应的Unity版本
Unity 2020.3 —— C# 8 
部分新内容还不在该版本Unity中被支持

### C#8的新增功能和语法有哪些
1. Using 声明
2. 静态本地函数
3. Null 合并赋值
4. 解构函数Deconstruct 
5. 模式匹配增强功能

### 静态本地函数
本地函数知识回顾
基本概念：在函数内部声明一个临时函数
注意：
本地函数只能在声明该函数的函数内部使用
本地函数可以使用声明自己的函数中的变量
作用：方便逻辑的封装
```c#
public int CalcInfo(int i)
{
    bool b = false;
    i += 10;
    Calc(ref i, ref b);
    return i;

    static void Calc(ref int i, ref bool b)
    {
        i += 10;
        b = true;
    }
}
```
新知识点：
静态本地函数就是在本地函数前方加入静态关键字
它的作用就是让本地函数不能够使用访问封闭范围内（也就是上层方法中）的任何变量
作用 让本地函数只能处理逻辑，避免让它通过直接改变上层变量来处理逻辑造成逻辑混乱

### Using 声明
知识回顾：
在数据持久化xml相关知识当中
我们学习了using相关的知识点
```c#
using(对象声明)
{
}
```
使用对象，语句块结束后 对象将被释放掉
当语句块结束 会自动帮助我们调用 对象的 Dispose这个方法 让其进行销毁
using一般都是配合 内存占用比较大 或者 有读写操作时  进行使用的 

```c#
using (StreamWriter stream = new StreamWriter("文件路径"))
{
    //对该变量进行逻辑处理 该变量只能在这个语句块中使用
    stream.Write(true);
    stream.Write(1.2f);
    stream.Flush();
    stream.Close();
}//语句块结束执行时 调用 声明对象的 Dispose方法 释放对象
```

新知识点：
Using 声明就是对using（）语法的简写
当函数执行完毕时 会调用 对象的 Dispose方法 释放对象

```c#
using StreamWriter s2 = new StreamWriter("文件路径");
//对该对象进行逻辑操作
s2.Write(5);
s2.Flush();
s2.Close();
```
利用这个写法 就会在上层语句块执行结束时释放该对象

注意：在使用using语法时，声明的对象必须继承System.IDisposable接口
因为必须具备Dispose方法，所以当声明没有继承该接口的对象时会报错
```c#
public class TestUsing : IDisposable
{
    public void Dispose() { }
}
using TestUsing t = new TestUsing();
```

### Null合并赋值
空合并赋值是C#8.0新加的一个运算符 **??=**
类似复合运算符
```c#
左边值 ??= 右边值
```
当左侧为空时才会把右侧值赋值给变量
举例：
```c#
str ??= "4565";
print(str);
//注意：由于左侧为空才会讲右侧赋值给变量，所以不为空的变量不会改变
str ??= "1111";
print(str);
```

### 解构函数Deconstruct
我们可以在自定义类当中声明解构函数
这样我们可以将该自定义类对象利用元组的写法对其进行变量的获取
语法：
在类的内部申明函数public void Deconstruct(out 变量类型 变量名, out 变量类型 变量名.....)
特点：
一个类中可以有多个Deconstruct，但是参数数量不能相同

```c#
Person p = new Person();
p.name = "唐老狮";
p.sex = false;
p.email = "tpandme@163.com";
p.number = "123123123123";

public class Person
{
    public string name;
    public bool sex;
    public string number;
    public string email;

    public void Deconstruct(out string n, out bool sex) => (n, sex) = (this.name, this.sex);
    public void Deconstruct(out string n, out bool sex, out string number) => (n, sex, number) = (this.name, this.sex, this.number);
    public void Deconstruct(out string n, out bool sex, out string number, out string email)
    {
        n = name;
        sex = this.sex;
        number = this.number;
        email = this.email;
    }
}

//我们可以对该对象利用元组将其具体的变量值 解构出来
//相当于把不同的成员变量拆分到不同的临时变量中
(string name, bool sex) = p;
print(name);
print(sex);
string str3;
(_, _, str3) = p;
print(str3);
```

### 模式匹配增强功能
#### switch表达式
switch表达式是对有返回值的switch语句的缩写
用=>表达式符号代替case:组合
用_弃元符号代替default
它的使用限制，主要是用于switch语句当中只有一句代码用于返回值时使用
语法：

函数声明 => 变量 switch
{
常量=>返回值表达式,
常量=>返回值表达式,
常量=>返回值表达式,
....
_ => 返回值表达式,
}

```c#
public Vector2 GetPos(PosType type) => type switch
{
    PosType.Top_Left => new Vector2(0, 0),
    PosType.Top_Right => new Vector2(1, 0),
    PosType.Bottom_Left => new Vector2(0, 1),
    PosType.Bottom_Right => new Vector2(1, 1),
    _ => new Vector2(0, 0)
};

public enum PosType
{
    Top_Left,
    Top_Right,
    Bottom_Left,
    Bottom_Right,
}
```

#### 属性模式
在常量模式的基础上判断对象上各属性
用法：变量 is {属性:值, 属性:值}
```c#
DiscountInfo info = new DiscountInfo("5折", true);
//if( info.discount == "6折" && info.isDiscount)
if (info is { discount: "6折", isDiscount: true })
    print("信息相同");

print(GetMoney(info, 100));

public class DiscountInfo
{
    public string discount;
    public bool isDiscount;

    public DiscountInfo(string discount, bool isDiscount)
    {
        this.discount = discount;
        this.isDiscount = isDiscount;
    }
}
```

它可以结合switch表达式使用
结合switch使用可以通过属性模式判断条件的组合

```c#
public float GetMoney(DiscountInfo info, float money) => info switch
{
    //可以利用属性模式 结合 switch表达式 判断n个条件是否满足
    { discount: "5折", isDiscount: true } => money * .5f,
    { discount: "6折", isDiscount: true } => money * .6f,
    { discount: "7折", isDiscount: true } => money * .7f,
    _ => money
};
```

#### 元组模式
通过刚才学习的 属性模式我们可以在switch表达式中判断多个变量同时满足再返回什么
但是它必须是一个数据结构类对象，判断其中的变量
而元组模式可以更简单的完成这样的功能，我们不需要声明数据结构类，可以直接利用元组进行判断
```c#
int ii = 10;
bool bb = true;
if ((ii, bb) is (11, true))
{
    print("元组的值相同");
}

//举例说明
public float GetMoney(string discount, bool isDiscount, float money) => (discount, isDiscount) switch
{
    ("5折", true) => money * .5f,
    ("6折", true) => money * .6f,
    ("7折", true) => money * .7f,
    _ => money
};

print(GetMoney("5折", true, 200));
```

#### 位置模式
如果自定义类中**实现了解构函数**
那么我们可以直接用对应类对象与元组进行is判断
```c#
DiscountInfo info = new DiscountInfo("5折", true);
if (info is ("5折", true))
{
    print("位置模式 满足条件");
}

public class DiscountInfo
{
    public string discount;
    public bool isDiscount;

    public DiscountInfo(string discount, bool isDiscount)
    {
        this.discount = discount;
        this.isDiscount = isDiscount;
    }

    public void Deconstruct(out string dis, out bool isDis)
    {
        dis = this.discount;
        isDis = this.isDiscount;
    }
}
```

同样我们也可以配合switch表达式来处理逻辑
```c#
public float GetMoney(DiscountInfo info, float money) => info switch
{
    ("5折", true) when money > 100 => money * .5f,
    ("6折", true) => money * .6f,
    ("7折", true) => money * .7f,
    _ => money
};
```
补充：配合when关键字进行逻辑处理
```c#
public float GetMoney(DiscountInfo info, float money) => info switch
{
    (string dis, bool isDis) when dis == "5折" && isDis => money * .5f,
    (string dis, bool isDis) when dis == "6折" && isDis => money * .6f,
    (string dis, bool isDis) when dis == "7折" && isDis => money * .7f,
    _ => money
};
```