**Canvas对象依附的：**
1. **Canvas：画布组件，主要用于渲染UI控件**
2. **Canvas Scaler：画布分辨率自适应组件，主要用于分辨率自适应**
3. **Graphic Raycaster：射线事件交互组件，主要用于控制射线响应相关**
4. **RectTransform：Ul对象位置锚点控制组件，主要用于控制位置和对其方式**

**EventSystem对象依附的：**
1. **EventSystem：玩家输入事件响应系统，主要用于监听玩家操作**
2. **Standalone Input Module：独立输入模块组件，主要用于监听玩家操作**

### Canvas

> **是UGUI中所有UI元素能够被显示的根本**
> 
> **主要负责渲染自己的所有UI子对象**
> 
> **通常一个场景只需要一个Canvas**

**Canvas三种渲染方式：**


**ScreenSpace-Overlay：Ul始终在前：**


![[ScreenSpace-Overlay.png]]


**ScreenSpace-Camera：3D物体可以显示在UI之前**


![[ScreenSpace-Camera.png]]


**World Space：3D模式**

### CanvasScaler

**CanvasScaler是用于分辨率自适应的组件，主要负责在不同分辨率下UI控件大小自适应**

**三种适配模式：**

**ConstantPixelSize(恒定像素模式)：无论屏幕大小如何，UI始终保持相同像素大小**


![[ConstantPixelSize.png]]
![[ConstantPixelSize2.png]]

它不会让UI控件进行分辨率大小自适应
会让UI控件始终保持设置的尺寸大小显示
一般在进行游戏开发极少使用这种模式
除非通过代码计算来设置缩放系数

**ScaleWithScreenSize(缩放模式)：根据屏幕尺寸进行缩放，随着屏幕尺寸放大缩小**


![[ScaleWithScreenSize.png]]
![[ScaleWithScreenSize2.png]]
![[ScaleWithScreenSize3.png]]
![[ScaleWithScreenSize4.png]]
![[ScaleWithScreenSize5.png]]


**ConstantPhysicalSize(恒定物理模式)：无论屏幕大小和分辨率如何，Ul元素始终保持相同物理大小**


![[ConstantPhysicalSize.png]]

它不会让UI控件进行分辨率大小自适应
会让UI控件始终保持设置的尺寸大小显示
而且会根据设备DPI进行计算，让在不同设备上的显示大小更加准确
一般在进行游戏开发极少使用这种模式

### Graphic Raycaster

**GraphicRaycaster用于检测UI输入事件的射线发射器**
**主要负责通过射线检测玩家和UI元素的交互，判断是否点击到了UI元素**

![[GraphicRaycaster.png]]

### EventSystem 和 Standalone Input Module

**EventSystem用于管理玩家的输入事件并分发给各UI控件**
**它类似一个中转站，和许多模块一起共同协作**
**如果没有它，所有点击、拖曳等等行为都不会被响应**

![[EventSystem.png]]

**Standalone Input Module主要针对处理鼠标/键盘/控制器/触屏（新版Unity）的输入**
**输入的事件通过EventSystem进行分发**

![[StandaloneInputModule.png]]


### RectTransform

**RectTransform继承于Transform，是专门用于处理UI元素位置大小相关的组件**
**RectTransform在Transform基础上加入了矩形相关，将UI元素当做一个矩形来处理**
**加入了中心点、锚点、长宽等属性，更加方便的控制其大小以及分辨率自适应中的位置适应**

![[RectTransform.png]]![[RectTransform2.png]]