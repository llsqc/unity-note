### 序列化
#### 声明类对象
如果要使用C#自带的序列化2进制方法，声明类时需要添加[System.Serializable]特性

```c#
[System.Serializable]
public class Person
{
    public int age = 1;
    public string name = "唐老狮";
    public int[] ints = new int[] { 1, 2, 3, 4, 5 };
    public List<int> list = new List<int>() { 1, 2, 3, 4 };
    public Dictionary<int, string> dic = new Dictionary<int, string>() { { 1, "123" }, { 2, "1223" }, { 3, "435345" } };
    public StructTest st = new StructTest(2, "123");
    public ClssTest ct = new ClssTest();
}

[System.Serializable]
public struct StructTest
{
    public int i;
    public string s;

    public StructTest(int i, string s)
    {
        this.i = i;
        this.s = s;
    }
}

[System.Serializable]
public class ClssTest
{
    public int i = 1;
}
```

#### 将对象进行2进制序列化
方法一：使用内存流得到2进制字节数组
主要用于得到字节数组 可以用于网络传输
新知识点
1. 内存流对象
类名：**MemoryStream**
命名空间：**System.IO**
2. 2进制格式化对象
类名：**BinaryFormatter**
命名空间：**System.Runtime.Serialization.Formatters.Binary**、
主要方法：序列化方法 **Serialize**

```c#
Person p = new Person();
using (MemoryStream ms = new MemoryStream())
{
    //2进制格式化程序
    BinaryFormatter bf = new BinaryFormatter();
    //序列化对象 生成2进制字节数组 写入到内存流当中
    bf.Serialize(ms, p);
    //得到对象的2进制字节数组
    byte[] bytes = ms.GetBuffer();
    //存储字节
    File.WriteAllBytes(Application.dataPath + "/Lesson5.tang", bytes);
    //关闭内存流
    ms.Close();
}
```

方法二：使用文件流进行存储
主要用于存储到文件中

```c#
using (FileStream fs = new FileStream(Application.dataPath + "/Lesson5_2.tang", FileMode.OpenOrCreate, FileAccess.Write))
{
    //2进制格式化程序
    BinaryFormatter bf = new BinaryFormatter();
    //序列化对象 生成2进制字节数组 写入到内存流当中
    bf.Serialize(fs, p);
    fs.Flush();
    fs.Close();
}
```

### 反序列化
#### 反序列化文件中数据
主要类
**FileStream**文件流类
**BinaryFormatter** 2进制格式化类
主要方法
**Deserizlize** 通过文件流打开指定的2进制数据文件

```c#
using (FileStream fs = File.Open(Application.dataPath + "/Lesson5_2.tang", FileMode.Open, FileAccess.Read))
{
    //声明一个 2进制格式化类
    BinaryFormatter bf = new BinaryFormatter();
    //反序列化
    Person p = bf.Deserialize(fs) as Person;

    fs.Close();
}
```

#### 反序列化网络传输过来的2进制数据
```c#
byte[] bytes = File.ReadAllBytes(Application.dataPath + "/Lesson5_2.tang");
//声明内存流对象 一开始就把字节数组传输进去
using (MemoryStream ms = new MemoryStream(bytes))
{
    //声明一个 2进制格式化类
    BinaryFormatter bf = new BinaryFormatter();
    //反序列化
    Person p = bf.Deserialize(ms) as Person;

    ms.Close();
}
```

### 加密
#### 常用加密算法
MD5算法
SHA1算法
HMAC算法
AES/DES/3DES算法

#### 用简单的异或加密感受加密的作用
```c#
//加密
Person p = new Person();
byte key = 199;
using (MemoryStream ms = new MemoryStream())
{
    BinaryFormatter bf = new BinaryFormatter();
    bf.Serialize(ms, p);
    byte[] bytes = ms.GetBuffer();
    //异或加密
    for (int i = 0; i < bytes.Length; i++)
    {
        bytes[i] ^= key;
    }
    File.WriteAllBytes(Application.dataPath + "/Lesson7.tang", bytes);
}
```

```c#
//解密
byte[] bytes2 = File.ReadAllBytes(Application.dataPath + "/Lesson7.tang");
for (int i = 0; i < bytes2.Length; i++)
{
    bytes2[i] ^= key;
}
using (MemoryStream ms = new MemoryStream(bytes2))
{
    BinaryFormatter bf = new BinaryFormatter();
    Person p2 = bf.Deserialize(ms) as Person;
    ms.Close();
}
```
