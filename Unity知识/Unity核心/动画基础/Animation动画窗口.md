
**打开Animation窗口**

Window——>Animation——>Animation

**Animation窗口作用**

Animation窗口主要用于在Unity内部创建和修改动画
所有在场景中的对象都可以通过Animation窗口为其制作动画

原理：
制作动画时：记录在固定时间点对象挂载的脚本的变量变化
播放动画时：将制作动画时记录的数据在固定时间点进行改变，产生动画效果

**关键词说明**

动画时间轴：
每一个动画文件都有自己的一个生命周期，从动画开始到结束
我们可以在动画时间轴上编辑每一个动画生命周期中变化

动画中的帧：
假设某个动画的帧率为60帧每秒，意味着该动画1秒钟最多会有60次改变机会
每一帧的间隔时间是 1s/60 ≈ 16.67毫秒
也就是说 我们最快可以每16.67毫秒改变一次对象状态

关键帧：
动画在时间轴上的某一个时间节点上处于的状态

![[认识Animation窗口 面板参数相关.png]]

### 创建编辑Animation动画

创建动画
1. 在场景中选中想要创建动画的对象
2. 在Animation窗口中点击创建
3. 选择动画文件将要保存到的位置

保存动画文件时，Unity会帮助我们完成以下操作
1. 创建一个 Animator Controller（动画控制器或称之为动画状态机） 资源（新动画系统）
2. 将新创建的动画文件添加到Animator Controller中
3. 为动画对象添加Animator组件
4. 为Animator组件关联创建的Animator Controller文件

![[创建编辑动画 参数相关.xmind]]

### 代码控制动画

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson31 : MonoBehaviour
{
    private Animation animation;

    void Start()
    {
        animation = this.GetComponent<Animation>();
        
        #region 知识点一 什么是老动画系统
        //Unity中有两套动画系统
        //新：Mecanim动画系统——主要用Animator组件控制动画
        //老：Animation动画系统——主要用Animation组件控制动画（Unity4之前的版本可能会用到）
        //目前我们为对象在Animation窗口创建的动画都会被新动画系统支配
        //有特殊需求或者针对一些简易动画，才会使用老动画系统
        #endregion

        #region 知识点二 老动画系统控制动画播放
        //注意：
        //在创建动画之前为对象添加Animation组件之后再制作动画
        //这时制作出的动画和之前的动画格式是有区别的
        #endregion

        #region 知识点四 动画事件
        //动画事件主要用于处理 当动画播放到某一时刻想要触发某些逻辑
        //比如进行伤害检测、发射子弹、特效播放等等
        #endregion
    }

    public void AnimationEvent()
    {
        print("动画事件触发");
    }

    void Update()
    {
		#region 知识点三 代码控制播放
        //1.播放动画
        animation.Play("1");
        
        //2.淡入播放,自动产生过渡效果
        
        //当你要播放的动画的开始状态 和当前的状态 不一样时 
        //就会产生过渡效果
        animation.CrossFade("3");
        
        //3.前一个播完再播放下一个
        
        animation.PlayQueued("2"); //无过度
        animation.CrossFadeQueued("2"); //有过度
        
        //4.停止播放所有动画
        animation.Stop();

        //5.是否在播放某个动画
        if( animation.IsPlaying("1") ) { }

        //6.播放模式设置
        animation.wrapMode = WrapMode.Loop;

        //7.其它（了解即可，新动画系统中会详细讲解）
        //层级和权重以及混合（老动画系统需要通过代码来达到动画的遮罩、融合等效果）
        
        //设置层级
        animation["1"].layer = 1;
        
        //设置权重
        animation["1"].weight = 1;
        
        //混合模式 叠加还是混合
        animation["1"].blendMode = AnimationBlendMode.Additive;
        
        //设置混组相关骨骼信息
        animation[""].AddMixingTransform();
        
        #endregion
    }
}
```

![[代码控制动画（老动画系统）知识点 参数相关.xmind]]

