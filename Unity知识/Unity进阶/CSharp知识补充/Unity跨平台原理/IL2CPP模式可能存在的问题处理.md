### IL2CPP打包存在的问题——类型裁剪

IL2CPP在打包时会自动对Unity工程的DLL进行裁剪，将代码中没有引用到的类型裁剪掉，
以达到减小发布后包的尺寸的目的。
然而在实际使用过程中，很多类型有可能会被意外剪裁掉，
造成运行时抛出找不到某个类型的异常。
特别是通过反射等方式在编译时无法得知的函数调用，在运行时都很有可能遇到问题

解决方案：
1. IL2CPP处理模式时，将PlayerSetting->Other Setting->Managed Stripping Level(代码剥离)设置为Low
	 Disable:Mono模式下才能设置为不删除任何代码
	 Low:默认低级别，保守的删除代码，删除大多数无法访问的代码，同时也最大程度减少剥离实际使用的代码的可能性
	 Medium:中等级别，不如低级别剥离谨慎，也不会达到高级别的极端
	 Hight:高级别，尽可能多的删除无法访问的代码，有限优化尺寸减小。如果选择该模式一般需要配合link.xml使用

2. 通过Unity提供的link.xml方式来告诉Unity引擎，哪些类型是不能够被剪裁掉的
	  在Unity工程的Assets目录中（或其任何子目录中）建立一个叫link.xml的XML文件


### IL2CPP打包存在的问题——泛型问题

我们上节课提到了IL2CPP和Mono最大的区别是 不能在运行时动态生成代码和类型
就是说 泛型相关的内容，如果你在打包生成前没有把之后想要使用的泛型类型显示使用一次
那么之后如果使用没有被编译的类型，就会出现找不到类型的报错

```
举例：List<A>和List<B>中A和B是我们自定义的类，
我能必须在代码中显示的调用过，IL2CPP才能保留List<A>和List<B>两个类型。
如果在热更新时我们调用List<C>，但是它之前并没有在代码中显示调用过，
那么这时就会出现报错等问题。主要就是因为JIT和AOT两个编译模式的不同造成的
        List<A> list = new List<A>();
        List<B> list2 = new List<B>();
```


解决方案：
	泛型类：声明一个类，然后在这个类中声明一些public的泛型类变量
	泛型方法：随便写一个静态方法，在将这个泛型方法在其中调用一下。这个静态方法无需被调用
	这样做的目的其实就是在预言编译之前让IL2CPP知道我们需要使用这个内容


### 总结
对于我们目前开发的新项目
都建议大家使用IL2CPP脚本后处理模式来进行打包
主要原因是因为它的效率相对Mono较高，同时由于它自带裁剪功能，包的大小也会小一些
但是如果在测试时出现 类型无法识别等问题
需要用到我们这节课学习的知识点来解决这些问题

**link.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
  
  <!--保存整个程序集-->
  <assembly fullname="UnityEngine" preserve="all"/>
  <!--没有“preserve”属性，也没有指定类型意味着保留所有-->
  <assembly fullname="UnityEngine"/>

  <!--完全限定程序集名称-->
  <assembly fullname="Assembly-CSharp, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null">
    <type fullname="Assembly-CSharp.Foo" preserve="all"/>
  </assembly>

  <!--在程序集中保留类型和成员-->
  <assembly fullname="Assembly-CSharp">
    <!--保留整个类型-->
    <type fullname="MyGame.A" preserve="all"/>
    <!--没有“保留”属性，也没有指定成员 意味着保留所有成员-->
    <type fullname="MyGame.B"/>
    <!--保留类型上的所有字段-->
    <type fullname="MyGame.C" preserve="fields"/>
    <!--保留类型上的所有方法-->
    <type fullname="MyGame.D" preserve="methods"/>
    <!--只保留类型-->
    <type fullname="MyGame.E" preserve="nothing"/>
    <!--仅保留类型的特定成员-->
    <type fullname="MyGame.F">
      <!--类型和名称保留-->
      <field signature="System.Int32 field1" />
      <!--按名称而不是签名保留字段-->
      <field name="field2" />
      <!--方法-->
      <method signature="System.Void Method1()" />
      <!--保留带有参数的方法-->
      <method signature="System.Void Method2(System.Int32,System.String)" />
      <!--按名称保留方法-->
      <method name="Method3" />

      <!--属性-->
      <!--保留属性-->
      <property signature="System.Int32 Property1" />
      <property signature="System.Int32 Property2" accessors="all" />
      <!--保留属性、其支持字段（如果存在）和getter方法-->
      <property signature="System.Int32 Property3" accessors="get" />
      <!--保留属性、其支持字段（如果存在）和setter方法-->
      <property signature="System.Int32 Property4" accessors="set" />
      <!--按名称保留属性-->
      <property name="Property5" />

      <!--事件-->
      <!--保存事件及其支持字段（如果存在），添加和删除方法-->
      <event signature="System.EventHandler Event1" />
      <!--根据名字保留事件-->
      <event name="Event2" />
    </type>

    <!--泛型相关保留-->
    <type fullname="MyGame.G`1">
      <!--保留带有泛型的字段-->
      <field signature="System.Collections.Generic.List`1&lt;System.Int32&gt; field1" />
      <field signature="System.Collections.Generic.List`1&lt;T&gt; field2" />

      <!--保留带有泛型的方法-->
      <method signature="System.Void Method1(System.Collections.Generic.List`1&lt;System.Int32&gt;)" />
      <!--保留带有泛型的事件-->
      <event signature="System.EventHandler`1&lt;System.EventArgs&gt; Event1" />
    </type>


    <!--如果使用类型，则保留该类型的所有字段。如果类型不是用过的话会被移除-->
    <type fullname="MyGame.I" preserve="fields" required="0"/>

    <!--如果使用某个类型，则保留该类型的所有方法。如果未使用该类型，则会将其删除-->
    <type fullname="MyGame.J" preserve="methods" required="0"/>

    <!--保留命名空间中的所有类型-->
    <type fullname="MyGame.SomeNamespace*" />

    <!--保留名称中带有公共前缀的所有类型-->
    <type fullname="Prefix*" />

  </assembly>
  
  

</linker>
```