### 日期和时间
在游戏开发当中我们经常会有和时间打交道的内容
比如每日签到、活动倒计时、建造时间、激活时间等等的功能
如果想要完成这些功能，仅仅用Unity提供给我们的Time类是远远不够用的
所以我们需要学习专门的日期和时间相关的知识
才能制作某些功能需求
而C#便提供了对应的结构方便我们处理时间相关逻辑
1. DateTime 日期结构体
2. TimeSpan 时间跨度结构体

#### 一些关于时间的名词说明
1s秒 = 1000ms毫秒
1ms毫秒 = 1000μs微妙
1μs微妙 = 1000ns纳秒

格里高利历:
格里高利历一般指公元

格林尼治时间(GMT)：
是指位于英国伦敦郊区的皇家格林尼治天文台的标准时间

协调世界时(UTC):
又称世界统一时间、世界标准时间、国际协调时间
UTC协调世界时即格林尼治平太阳时间，是指格林尼治所在地的标准时间，
也是表示地球自转速率的一种形式
UTC基于国际原子时间，通过不规则的加入闰秒来抵消地球自转变慢的影响，是世界上调节时钟和时间的主要时间标准

时间戳
从1970年1月1日（UNIX时间戳的起点时间）到现在的时间
计算机时间和众多的编程语言的时间都是从1970年1月1日开始算起

#### DateTime
命名空间：**System**
DateTime 是 C# 提供给我们处理日期和时间的结构体
DateTime 对象的默认值和最小值是0001年1月1日00:00:00（午夜）
        最大值可以是9999年12月31日晚上11:59:59

##### 初始化
主要参数：
年、月、日、时、分、秒、毫秒
**ticks**：以格里高利历00:00:00.000年1月1日以来的100纳秒间隔数表示,一般是一个很大的数字
次要参数：
**DateTimeKind**：日期时间种类
**Local**：本地时间
**Utc**：UTC时间
**Unspecified**：不指定
**Calendar**:日历
使用哪个国家的日历，一般在Unity开发中不使用
```c#
DateTime dt = new DateTime(2022, 12, 1, 13, 30, 45, 500);
```
年、月、日、时、分、秒、毫秒
```c#
print(dt.Year + "-" + dt.Month + "-" + dt.Day + "-" + dt.Hour + "-" + dt.Minute + "-" + dt.Second + "-" + dt.Millisecond);
```
以格里高利历00:00:00.000年1月1日以来的100纳秒间隔数表示,一般是一个很大的数字
```c#
print(dt.Ticks);
```
一年的第多少天
```c#
print(dt.DayOfYear);
```
星期几
```c#
print(dt.DayOfWeek);
```

##### 获取时间
当前日期和时间
```c#
DateTime nowTime = DateTime.Now;
print(nowTime.Minute);
```
返回今日日期
```c#
DateTime nowTime2 = DateTime.Today;
print(nowTime2.Year + "-" + nowTime2.Month + "-" + nowTime2.Day);
```
返回当前UTC日期和时间
```c#
DateTime nowTimeUTC = DateTime.UtcNow;
```

##### 计算时间
各种加时间
```c#
DateTime nowTime3 = nowTime.AddDays(-1);
print(nowTime3.Day);
```

##### 字符串输出
```c#
print(nowTime.ToString());
print(nowTime.ToShortTimeString());
print(nowTime.ToShortDateString());
print(nowTime.ToLongTimeString());
print(nowTime.ToLongDateString());

print(nowTime.ToString("D"));
print(nowTime.ToString("yyyy-MM-dd-ddd/HH-mm-ss"));
```

##### 字符串转DateTime
字符串想要转回DateTime成功的话 
那么这个字符串的格式是有要求的 一定是最基本的 toString的转换出来的字符串才能转回去
年/月/日 时:分:秒
```c#
string str = nowTime.ToString();
str = "1988/5/4 18:00:08";
print(str);
DateTime dt3;
if (DateTime.TryParse(str, out dt3))
{
    print(dt3);
}
else
{
    print("转换失败");
}
```

##### 存储时间
1. 以直接存字符串
2. 可以直接存Ticks
3. 可以直接存时间戳信息

存储时间戳的形式 更加节约

#### TimeSpan
命名空间：**System**
TimeSpan 是 C# 提供给我们的时间跨度结构体
用两个DateTime对象相减 可以得到该对象
```c#
TimeSpan ts = DateTime.Now - new DateTime(1970, 1, 1);
print(ts.TotalMinutes);
print(ts.TotalSeconds);
print(ts.TotalDays);
print(ts.TotalHours);
print(ts.Ticks);

print(ts.Days + "-" + ts.Hours + "-" + ts.Minutes + "-" + ts.Seconds + "-" + ts.Milliseconds);
```

##### 初始化它来代表时间间隔
```c#
TimeSpan ts2 = new TimeSpan(1, 0, 0, 0);
DateTime timeNow = DateTime.Now + ts2;
```

##### 相互计算
```c#
TimeSpan ts3 = new TimeSpan(0, 1, 1, 1);
TimeSpan ts4 = ts2 + ts3;
print(ts4.Days + "-" + ts4.Hours);
```

##### 自带常量方便用于和ticks进行计算
```c#
print(ts4.Ticks / TimeSpan.TicksPerSecond);
```