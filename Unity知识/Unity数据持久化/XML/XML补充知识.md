### XML序列化

```c#
using System.Collections;
using System.Collections.Generic;
using System.IO;
using System.Xml.Serialization;
using UnityEngine;

public class Lesson1Test
{
    [XmlElement("testPublic123123")]
    public int testPublic;
    private int testPrivate;
    protected int testProtected;
    internal int testInternal;

    public string testPUblicStr;

    public int testPro { get; set; }

    public Lesson1Test2 testClass = new Lesson1Test2();

    public int[] arrayInt;
    [XmlArray("IntList")]
    [XmlArrayItem("Int32")]
    public List<int> listInt;
    public List<Lesson1Test2> listItem;

    //不支持字典
    //public Dictionary<int, string> testDic = new Dictionary<int, string>() { { 1, "123" } };
}

public class Lesson1Test2
{
    [XmlAttribute("Test1")]
    public int test1 = 1;
    [XmlAttribute()]
    public float test2 = 1.1f;
    [XmlAttribute()]
    public bool test3 = true;
}


public class Lesson1 : MonoBehaviour
{
    void Start()
    {
        #region 知识点一 什么是序列化和反序列化
        //序列化：把对象转化为可传输的字节序列过程称为序列化
        //反序列化：把字节序列还原为对象的过程称为反序列化

        //说人话：
        //序列化就是把想要存储的内容转换为字节序列用于存储或传递
        //反序列化就是把存储或收到的字节序列信息解析读取出来使用
        #endregion

        #region 知识点二 xml序列化
        //1.第一步准备一个数据结构类
        Lesson1Test lt = new Lesson1Test();
        //2.进行序列化
        //  关键知识点
        //  XmlSerializer 用于序列化对象为xml的关键类
        //  StreamWriter 用于存储文件  
        //  using 用于方便流对象释放和销毁

        //第一步：确定存储路径
        string path = Application.persistentDataPath + "/Lesson1Test.xml";
        print(Application.persistentDataPath);
        //第二步：结合 using知识点 和 StreamWriter这个流对象 来写入文件
        // 括号内的代码：写入一个文件流 如果有该文件 直接打开并修改 如果没有该文件 直接新建一个文件
        // using 的新用法 括号当中包裹的声明的对象 会在 大括号语句块结束后 自动释放掉 
        // 当语句块结束 会自动帮助我们调用 对象的 Dispose这个方法 让其进行销毁
        // using一般都是配合 内存占用比较大 或者 有读写操作时  进行使用的 
        using ( StreamWriter stream = new StreamWriter(path) )
        {
            //第三步：进行xml文件序列化
            XmlSerializer s = new XmlSerializer(typeof(Lesson1Test));
            
            //这句代码的含义 就是通过序列化对象 对我们类对象进行翻译 将其翻译成我们的xml文件 写入到对应的文件中
            //第一个参数 ： 文件流对象
            //第二个参数: 想要备翻译 的对象
            //注意 ：翻译机器的类型 一定要和传入的对象是一致的 不然会报错
            s.Serialize(stream, lt);
        }
        #endregion

        #region 知识点三 自定义节点名 或 设置属性
        //可以通过特性 设置节点或者设置属性 并且修改名字
        #endregion

        #region 总结
        //序列化流程
        //1.有一个想要保存的类对象
        //2.使用XmlSerializer 序列化该对象
        //3.通过StreamWriter 配合 using将数据存储 写入文件
        //注意：
        //1.只能序列化公共成员
        //2.不支持字典序列化
        //3.可以通过特性修改节点信息 或者设置属性信息
        //4.Stream相关要配合using使用
        #endregion
    }
}
```

### XML反序列化

```c#
using System.Collections;
using System.Collections.Generic;
using System.IO;
using System.Xml.Serialization;
using UnityEngine;

public class Lesson2 : MonoBehaviour
{
    void Start()
    {
        #region 知识回顾
        // 序列化 就是把类对象 转换为可存储和传输的数据
        // 反序列化 就是把存储或收到的数据 转换为 类对象

        // xml序列化关键知识
        // 1.using 和 StreamWriter
        // 2.XmlSerializer 的 Serialize序列化方法
        #endregion

        #region 知识点一 判断文件是否存在
        string path = Application.persistentDataPath + "/Lesson1Test.xml";
        if( File.Exists(path) )
        {
            #region 知识点二 反序列化
            //关键知识
            // 1.using 和 StreamReader
            // 2.XmlSerializer 的 Deserialize反序列化方法

            //读取文件
            using (StreamReader reader = new StreamReader(path))
            {
                //产生了一个 序列化反序列化的翻译机器
                XmlSerializer s = new XmlSerializer(typeof(Lesson1Test));
                Lesson1Test lt = s.Deserialize(reader) as Lesson1Test;
            }
            #endregion
        }
        #endregion
}

```

### IXmlSerializable接口

```c#
using System.Collections;
using System.Collections.Generic;
using System.IO;
using System.Xml;
using System.Xml.Schema;
using System.Xml.Serialization;
using UnityEngine;

public class TestLesson3 : IXmlSerializable
{
    public int test1 = 10;
    public int test2 = 99;

    public XmlSchema GetSchema()
    {
        return null;
    }

    public void ReadXml(XmlReader reader)
    {
        //读属性
        test1 = int.Parse(reader["Test1"]);
        test2 = int.Parse(reader["Test2"]);

        //读节点
        //方式一
        reader.Read();//这时读到的是节点
        reader.Read();//这时读到的才是值
        test1 = int.Parse(reader.Value);//得到值内容
        reader.Read();//得到节点尾部配对
        reader.Read();//读到节点开头
        reader.Read();//读到值
        test2 = int.Parse(reader.Value);//获取值内容
        //方式二
        while (reader.Read())
        {
            if(reader.NodeType == XmlNodeType.Element)
            {
                switch (reader.Name)
                {
                    case "Test1":
                        reader.Read();
                        test1 = int.Parse(reader.Value) ;
                        break;
                    case "Test2":
                        reader.Read();
                        test2 = int.Parse(reader.Value);
                        break;
                }
            }
        }

        //读包裹点
        XmlSerializer s = new XmlSerializer(typeof(int));
        reader.Read();
        reader.ReadStartElement("Test1");
        test1 = (int)s.Deserialize(reader);
        reader.ReadEndElement();
        reader.ReadStartElement("Test2");
        test1 = (int)s.Deserialize(reader);
        reader.ReadEndElement();
    }

    public void WriteXml(XmlWriter writer)
    {
        //写属性
        writer.WriteAttributeString("Test1", test1.ToString());
        writer.WriteAttributeString("Test2", test2.ToString());

        //写节点
        writer.WriteElementString("Test1", test1.ToString());
        writer.WriteElementString("Test2", test2.ToString());

        //写包裹节点
        XmlSerializer s = new XmlSerializer(typeof(int));
        writer.WriteStartElement("Test1");
        s.Serialize(writer, test1);
        writer.WriteEndElement();

        writer.WriteStartElement("Test2");
        s.Serialize(writer, test2);
        writer.WriteEndElement();

    }
}

public class Lesson3 : MonoBehaviour
{
    void Start()
    {
        #region 知识点一 IXmlSerializable是什么
        //C# 的XmlSerializer 提供了可拓展内容 
        //可以让一些不能被序列化和反序列化的特殊类能被处理
        //让特殊类继承 IXmlSerializable 接口 实现其中的方法即可
        #endregion

        #region 知识点二 自定义类实践
        TestLesson3 t = new TestLesson3();
        using (StreamWriter writer = new StreamWriter(Application.persistentDataPath + "/test.xml"))
        {
            XmlSerializer s = new XmlSerializer(typeof(TestLesson3));
            s.Serialize(writer, t);
        }

        using(StreamReader reader = new StreamReader(Application.persistentDataPath + "/test.xml"))
        {
            XmlSerializer s = new XmlSerializer(typeof(TestLesson3));
            t = s.Deserialize(reader) as TestLesson3;
        }
        #endregion
    }
}

```

### 让Dictionary支持序列化和反序列化

```c#
using System.Collections;
using System.Collections.Generic;
using System.Xml;
using System.Xml.Schema;
using System.Xml.Serialization;
using UnityEngine;

public class SerizlizerDictionary<TKey, TValue> : Dictionary<TKey, TValue>, IXmlSerializable
{
    public XmlSchema GetSchema()
    {
        return null;
    }

    //自定义字典的 反序列化 规则
    public void ReadXml(XmlReader reader)
    {
        XmlSerializer keySer = new XmlSerializer(typeof(TKey));
        XmlSerializer valueSer = new XmlSerializer(typeof(TValue));

        //要跳过根节点
        reader.Read();
        //判断 当前不是元素节点 结束 就进行 反序列化
        while (reader.NodeType != XmlNodeType.EndElement)
        {
            //反序列化键
            TKey key = (TKey)keySer.Deserialize(reader);
            //反序列化值
            TValue value = (TValue)valueSer.Deserialize(reader);
            //存储到字典中
            this.Add(key, value);
        }
        reader.Read();
    }

    //自定义 字典的 序列化 规则
    public void WriteXml(XmlWriter writer)
    {
        XmlSerializer keySer = new XmlSerializer(typeof(TKey));
        XmlSerializer valueSer = new XmlSerializer(typeof(TValue));

        foreach (KeyValuePair<TKey, TValue> kv in this)
        {
            //键值对 的序列化
            keySer.Serialize(writer, kv.Key);
            valueSer.Serialize(writer, kv.Value);
        }
    }
}
```