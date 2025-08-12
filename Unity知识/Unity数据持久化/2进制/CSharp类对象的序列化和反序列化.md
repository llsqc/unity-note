### 序列化

#### 申明类对象
如果要使用C#自带的序列化2进制方法，申明类时需要添加[System.Serializable]特性

#### 将对象进行2进制序列化

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