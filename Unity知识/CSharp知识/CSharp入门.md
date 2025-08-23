### 控制台输入
```C#
Console.WriteLine();
Console.Write();
Console.ReadLine();
Console.ReadKey();

// 不会把输入的内容显示
char input = Console.ReadKey(true).KeyChar;

// 清空控制台
Console.Clear();

// 检测键盘是否读入输入
// 获取输入时为true，否则为false
Console.KeyAvailable
```

```c#
// 1. 设置控制台大小
// 窗口大小
Console.SetWindowSize(100,50);

// 缓冲区大小
Console.SetBufferSize(1000, 1000);

// 2. 设置光标位置, 视觉上1x = 2y
Console.SetCursorPosition(0, 1);

// 3. 设置颜色相关
// 文字颜色设置
Console.ForegroundColor = ConsoleColor.Red;

// 背景颜色设置
Console.BackgroundColor = ConsoleColor.Red;
Console.Clear();

// 4. 光标显示隐藏
Console.CursorVisible = false;

// 5. 关闭控制台
Environment.Exit(0);
```

### 变量
```C#
//折叠代码
#region MyRegion

#endregion

//变量
// sbyte -128~127
// byte 0~255
// uint ushort ulong
// float double (decimal)
float f = 1.23456789f;
double d = 0.1234567890123456789;
decimal de = 0.1234567890123456789012345678901234567890m;
```

### 转义字符
```C#
//取消转义字符  @
string str = @"Hello\World";
```

### 显式转换
```c#
//括号强制转换
long i = (int)30;

// Parse , 将string转化成其他
int i = int.Parse("123");

// Convert , 准确转换各个类型
int a = Convert.ToInt32("12");
```

### 异常捕获
```c#
try { }
catch(Exception e) { }
finally { }
```

### 字符串拼接
```c#
// + 直接拼接
// Format
string.Format();
```

### 随机数
```c#
Random r = new Random();

int i = r.Next(); //生成非负随机数
int i = r.Next(100); // 0~99
int i = r.Next(5,100) // 5~99
```
