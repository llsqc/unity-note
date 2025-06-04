### XML基本语法

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- XML声明，版本和编码格式-->

<!-- 必须需要一个根节点-->
<!-- <节点名>填写数据或其他节点</节点名>-->
<PlayerInfo>
    <Player>
        <Name>John Doe</Name>
        <Score>1500</Score>
        <Level>10</Level>
    </Player>
    <Player>
        <Name>Jane Smith</Name>
        <Score>2000</Score>
        <Level>15</Level>
    </Player>
</PlayerInfo>

<!-- XML基本规则

1.每个元素都必须有关闭标签
2.元素命名规则基本遵照C#中变量名命名规则
3.XML标签对大小写敏感
4.XML文档必须有根元素
5.特殊的符号应该用实体引用

&lt  小于 <
&gt  大于 >
&amp  和号 &
&apos 单引号 '
&quot 双引号 "

-->
```

### XML属性

```xml
<?xml version="1.0" encoding="UTF-8"?>
<PlayerInfo>
    <!-- 使用元素标签嵌套表示信息-->
    <Player>Player1
        <Name>John Doe</Name>
        <Score>1500</Score>
        <Level>10</Level>
    </Player>

    <!-- 使用属性表示信息-->
    <Player Name = "Jane Smith" Score = "2000" Level = "15" >Player2</Player>

    <!-- 不需要元素记录-->
    <Player Name = "Bob Johnson" Score = "1200" Level = "8" />
</PlayerInfo>
```

