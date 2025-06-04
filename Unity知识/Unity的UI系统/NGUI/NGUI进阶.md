### EventListener和EventTrigger

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson14 : MonoBehaviour
{
    public UISprite A;
    public UISprite B;
    
    void Start()
    {
        #region 知识点二 NGUI事件 响应函数
        //添加了碰撞器的对象
        //NGUI提供了一些利用反射调用的函数
        //经过 OnHover(bool isOver)
        //按下 OnPress(bool pressed)
        //点击 OnClick()
        //双击 OnDoubleClick()
        //拖曳开始 OnDragStart()
        //拖曳中  OnDrag(Vector2 delta)
        //拖曳结束 OnDragEnd()
        //拖曳经过某对象 OnDragOver(GameObject go)
        //拖曳离开某对象 OnDragOut(GameObject go)
        //等等
        #endregion
        
        #region 知识点三 更加方便的UIEventListener和UIEventTrigger
        //他们帮助我们封装了所有 特殊响应函数
        //可以通过它进行管理添加
        
        //1.UIEventListener 适合代码添加
        UIEventListener listener = UIEventListener.Get(A.gameObject);
        listener.onPress += (obj, isPress) => {
            print(obj.name + "被按下或者抬起了" + isPress);
        };
        
        //2.UIEventTrigger 适合Inspector面板 关联脚本添加
        
        //UIEventListener和UIEventTrigger区别
        //1.Listener更适合 代码添加监听 Trigger适合拖曳对象添加监听
        //2.Listener传入的参数 更具体  Trigger就不会传入参数 我们需要在函数中去判断处理逻辑
        #endregion
    }
}
```

### DrawCall

**DrawCall 概念**

就是CPU(处理器)准备好渲染数据（顶点，纹理，法线，Shader等等）后
告知GPU(图形处理器-显卡)开始渲染（将命令放入命令缓冲区）的命令

简单来说：一次DrawCall就是 CPU准备好渲染数据通知 GPU渲染的这个过程

如果游戏中DrawCall数量较高会影响CPU的效率，最直接的感受就是游戏会卡顿

每次DrawCall，CPU都需要准备很多数据发送给GPU
如果DrawCall 太多CPU就会把大量时间花在提交DrawCall上 造成CPU过载，游戏卡顿

**如何降低DrawCall数量**

在UI层面上
小图合大图——>即多个小DrawCall变一次大DrawCall

**制作UI时降低DrawCall的技巧**

1. 通过NGUI Panel上提供的DrawCall查看工具
2. 注意不同图集之间的层级关系
3. 注意Label的层级关系

### NGUI字体

**NGUI字体的作用**

1. 降低DrawCall
2. 自定义美术字体

**制作NGUI字体**

NGUI内部提供了字体制作工具
1. 根据字体文件 生成指定内容文字 达到降低DrawCall的目的
2. 使用第三方工具BitmapFont生成字体信息和图集

**Unity动态字体和NGUI字体如何选择**

1. 文字变化较多用Unity动态字体 变化较少用NGUI字体
2. 想要减少DrawCall用NGUI字体
3. 美术字用NGUI字体

### NGUI缓动

**什么是NGUI缓动**

就是让控件交互时进行缩放变换、透明变换、位置变换、角度变换等行为
NGUI自带Tween功能来实现这些缓动效果

**如何使用NGUI缓动**

1. Tween缓动相关组件
2. PlayTween让对象和输入事件相关联

![[Tween关键参数.bmp]]

![[PlayTween关键参数.bmp]]

![[PlayTween参数.bmp]]

### NGUI中显示3D模型和粒子特效

**NGUI中显示3D模型**

方法一：
使用UI摄像机渲染3D模型
1. 改变NGUI的整体层级 为 UI层
2. 改变主摄像机和NGUI摄像机的 渲染层级
3. 将想要被UI摄像机渲染的对象层级改为 UI层
4. 调整模型和UI控件的Z轴距离

方法二：
使用多摄像机渲染 Render Texture

**NGUI中显示粒子特效**

1. 让Panel和粒子特效处于一个排序层
2. 在粒子特效的 Render参数中 设置自己的层级

### 其他功能

#### NGUI事件响应、播放音效

**PlaySound脚本**

#### NGUI控件和键盘按键绑定

**KeyBinding脚本**

#### PC端Tab键快捷切换选中

**KeyNavigation脚本**

#### 语言本地化

**Localization脚本**

1. 在Resources下创建一个txt文件 命名必须为Localization
2. 配置文件
3. 给想要切换文字的Label对象下挂载Localize 关联Key
4. 给用于切换语言的下拉列表下添加脚本LanguageSelection