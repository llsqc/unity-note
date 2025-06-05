### 有限状态机

什么是有限状态机
有限状态机（Finite - state machine, FSM）又称有限状态自动机，简称状态机
是表示有限个状态以及在这些状态之间的转移和动作等行为的数学模型

游戏开发中有很多功能系统都是有限状态机
最典型的状态机系统：
动作系统 —— 当满足某个条件切换一个动作，且动作是有限的
AI系统 —— 当满足某个条件切换一个状态，且状态时有限的

### Animator Controller

创建动画状态机
1. 通过为场景中物体创建动画时自动创建
2. 手动创建动画状态机文件

基础使用：
1. 添加动画：
	1. 自动添加——为对象创建动画后会自动将动画添加到状态机中
	2. 手动添加1——将动画文件拖入到状态机中（注意：老动画拖入会有警告）
	3. 手动添加2——右键创建状态，再关联动画
2. 添加切换连线
3. 添加切换条件
	1. 在左侧面板点击参数页签
	2. 可以在这里添加4种类型的切换条件
4. 设置动画间切换条件

![[状态机参数相关.png]]

### 代码控制状态机

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson34 : MonoBehaviour
{
    private Animator animator;

    void Start()
    {
        #region 知识点一 关键组件Animator
        //参数相关
        #endregion
        
        #region 知识点二 Animator中的API
        //我们用代码控制状态机切换主要使用的就是Animator提供给我们的API
        //我们知道一共有四种切换条件 int float bool trigger
        //所以对应的API也是和这四种类型有关系的
        animator = this.GetComponent<Animator>();
        //1.通过状态机条件切换动画
        animator.SetFloat("条件名", 1.2f);
        animator.SetInteger("条件名", 5);
        animator.SetBool("条件名", true);
        animator.SetTrigger("条件名");
        
        animator.GetFloat("条件名");
        animator.GetInteger("条件名");
        animator.GetBool("条件名");
        
        //2.直接切换动画 除非特殊情况 不然一般不使用
        animator.Play("状态名");
        #endregion
    }
    
    void Update()
    {
        if( Input.GetKeyDown(KeyCode.A) ){
            animator.SetBool("change", true);
        }
        
        if(Input.GetKeyDown(KeyCode.S)){
            animator.SetBool("change", false);
        }
    }
}
```

![[代码控制动画状态机切换 知识点参数.png]]