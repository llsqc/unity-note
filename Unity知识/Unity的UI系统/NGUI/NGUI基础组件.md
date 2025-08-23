### Root组件(UIRoot)
![[Flexible.png]]

![[Constrained.png]]

![[缩放模式设置.bmp]]

1. Flexible 适用于可以手动拖窗口改变分辨率的设备 比如pc端

2. Constrained 适用于移动设备
	因为移动设备都是全屏应用 不会频繁改变分辨率 只用适配不同分辨率的设备
	横屏勾选 高 fit  竖屏 勾选 宽 fit 一般就可以比较好的进行分辨率适应了 
	需要注意的是 背景图 一定要考虑 极限 宽高比来出 最大宽高比  19.9:9

3. Constrained On Mobiles 是上面两者的综合体 适用于多平台发布的游戏和应用

### Panel组件(UIPanel)
作用：
1. 管理一个UI面板的渲染顺序
2. 管理一个UI面板上的所有子控件

![[重要参数.bmp]]


![[次要参数.bmp]]

总结:
1. 没有Panel父对象 UI控件看不到
2. Panel一般用于管理面板 控制层级
3. Panel可以有多个 一般一个Panel管理一个面板

### EventSystem组件(UICamera)
EventSystem作用：
1. 主要作用是让摄像机渲染出来的物体
2. 能够接收到NGUI的输入事件
3. 大部分设置不需要我们去修改
4. 有了它我们通过鼠标 触碰 键盘 控制器 操作UI 响应玩家的输入

![[Sprite参数1.bmp]]

![[Sprite参数2.bmp]]

总结：
1. EventSystem很重要，如果没有它，我们没有办法监听玩家输入
2. 创建UI时的 2DUI 和3DUI 主要就是摄像机的模式不一样
	EventSystem的2D和3D主要是 采用2D碰撞器 还是3D碰撞器 不能直接改变摄像机模式