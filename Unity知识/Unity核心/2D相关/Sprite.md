### Sprite Editor
#### Single图片编辑
**SpriteEditor是什么**
SpriteEditor主要用于编辑2D游戏开发中使用的Sprite精灵图片
它可以用于编辑 图集中提取元素，设置精灵边框，设置九宫格，设置轴心（中心）点等等功能

**安装 2D Sprite**
新版本Unity 需要安装 2D Sprite包才能使用SpriteEditor

**Single图片编辑**
Single图片编辑主要讲解的就是在设置图片时
将精灵图片模式（Sprite Mode）设置为Single的精灵图片在Sprite Editor窗口中如何编辑

1. Sprite Editor
  基础图片设置（右下角窗口）
  主要用于设置单张图片的基础属性

2. Custom Outline（决定渲染区域）
  自定义边缘线设置，可以自定义精灵网格的轮廓形状
  默认情况下不修改都是在矩形网格上渲染,边缘外部透明区域会被渲染，浪费性能
  使用自定义轮廓，可以调小透明区域，提高性能

3. Custom Physics Shape（决定碰撞判断区域）
  自定义精灵图片的物理形状，主要用于设置需要物理碰撞判断的2D图形
  它决定了之后产生碰撞检测的区域

4. Secondary Textures(为图片添加特殊效果)
  次要纹理设置，可以将其它纹理和该精灵图片关联
  着色器可以得到这些辅助纹理然后用于做一些效果处理
  让精灵应用其它效果

![[Single图片编辑 参数相关.png]]

#### Multiple图集元素分割
当图片资源是图集时，需要在设置时将模式设置为Multiple
这时我们可以使用Sprite Editor自带的功能进行图集元素分割

![[Multiple图片编辑 参数相关.png]]

#### Polygon多边形编辑
如果我们使用的资源是多边形资源，可以在设置时将模式设置为Polygon，然后可以在SpriteEditor中进行快捷设置
但是一般这种模式在实际开发中使用较少

### Sprite Renderer
Sprite Renderer是精灵渲染器，所有2D游戏中游戏资源（除UI外）都是通过Sprite Renderer让我们看到的
它是2D游戏开发中的一个极为重要的组件

2D对象创建的方法
1. 直接拖入Sprite图片
2. 右键创建
3. 空物体添加脚本

参数讲解：

![[Sprite Renderer参数相关.xmind]]

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson10 : MonoBehaviour
{
    void Start()
    {
        GameObject obj = new GameObject();
        SpriteRenderer sr = obj.AddComponent<SpriteRenderer>();
        //动态的改变图片
        sr.sprite = Resources.Load<Sprite>("dead1");
        //动态的加载 图集中的图
        Sprite[] sprs = Resources.LoadAll<Sprite>("RobotBoyIdleSprite");
        sr.sprite = sprs[10];
        
        print(sprs[10].name);
    }
}
```

### Sprite Creator
Sprite Creator是什么？

Sprite Creator是精灵创造者，我们可以利用Sprite Editor的多边形工具创造出各种多边形
Unity也为我们提供了现成的一些多边形

它的主要作用是2D游戏的替代资源，在等待美术出资源时我们可以用他们作为替代品，有点类似Unity提供的自带几何体

知识点二 使用Sprite Creator
在Project窗口右键创建各种形状的Sprite精灵图片

### Sprite Mask
顾名思义，SpriteMask是精灵遮罩的意思
它的主要作用就是对精灵图片产生遮罩
制作一些特殊的功能，比如只显示图片的一部分让玩家看到

![[SpriteMask 参数相关.png]]

### Sorting Group
SortingGroup是什么？
顾名思义，SortingGroup是排序分组的意思
它的主要作用就是对多个Sprite进行分组排序
Unity会将同一个排序组中的Sprite一起排序，就好像他们是单个游戏对象一样
主要作用是对于需要分层的2D游戏用于整体排序


**注意事项**
1. 子排序组，先排子对象，再按父对象和别人一起排（同层和同层比）
2. 多个挂载排序分组组件的预设体之间，通过修改排序索引号来决定前后顺序

**使用场景**
2D游戏的前景后景等

### Sprite Atlas
在工程设置面板中打开功能
Edit——>Project Setting——>Editor——>Sprite Packer

**Disabled**：默认设置，不会打包图集
**Enabled For Builds**（Legacy Sprite Packer）：Unity仅在构建时打包图集，在编辑模式下不会打包图集
**Always Enabled**（Legacy Sprite Packer）：Unity在构建时打包图集，在编辑模式下运行前会打包图集

**Legacy Sprite Packer传统打包模式** 相对下面两种模式来说 多了一个设置图片之间的间隔距离
**Padding Power**:选择打包算法在计算打包的精灵之间以及精灵与生成的图集边缘之间的间隔距离，这里的数字代表2的n次方

**Enabled For Build**：Unity进在构建时打包图集，在编辑器模式下不会打包
**Always Enabled**：Unity在构建时打包图集，在编辑模式下运行前会打包图集

![[SpriteAtlas参数相关.png]]

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.U2D;

public class Lesson14 : MonoBehaviour
{
    void Start()
    {
        GameObject obj = new GameObject();
        SpriteRenderer sr = obj.AddComponent<SpriteRenderer>();
        //加载图集资源
        SpriteAtlas spriteAtlas = Resources.Load<SpriteAtlas>("MyAtlas");
        //加载图集资源中的某一张小图
        sr.sprite = spriteAtlas.GetSprite("dead1");
        #endregion
    }
}
```