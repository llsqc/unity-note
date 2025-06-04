
**如何进行IK控制**
1. 在状态机的层级设置中 开启 IK 通道
2. 继承MonoBehavior的类中，Unity定义了一个IK回调函数:**OnAnimatorIK**
	可以在该函数中调用Unity提供的IK相关API来控制IK
3. Animator中的IK相关API
  **SetLookAtWeight**     设置头部IK权重
  **SetLookAtPosition**   设置头部IK看向位置
  **SetIKPositionWeight** 设置IK位置权重
  **SetIKRotationWeight** 设置IK旋转权重
  **SetIKPosition**       设置IK对应的位置
  **SetIKRotation**       设置IK对应的角度
  **AvatarIKGoal**    四肢末端IK枚举

**关于 OnAnimatorIK 和 OnAnimatorMove 两个函数的理解**
这两个函数是两个和动画相关的特殊生命周期函数
在Update之后，LateUpdate之前调用，在每帧的状态机和动画处理完后调用
**OnAnimatorIK**在**OnAnimatorMove之前**调用
**OnAnimatorIK**主要处理 **IK运动相关逻辑**
**OnAnimatorMove**主要处理 **动画移动以修改根运动的回调逻辑**
他们存在的目的只是多了一个调用时机，当每帧的动画和状态机逻辑处理完后再调用


```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson55 : MonoBehaviour
{
    private Animator animator;

    public Transform pos;
    public Transform pos2;

    void Start()
    {   
        animator = this.GetComponent<Animator>();
    }
    
    private void OnAnimatorIK(int layerIndex)
    {
        //头部IK相关
        //weight:LookAt全局权重0~1
        //bodyWeight:LookAt时身体的权重0~1
        //headWeight:LookAt时头部的权重0~1
        //eyesWeight:LookAt时眼镜的权重0~1
        //clampWeight:0表示角色运动时不受限制，1表示角色完全固定无法执行LookAt，0.5表示只能够移动范围的一半
        animator.SetLookAtWeight(1, 1f, 1f);
        animator.SetLookAtPosition(pos.position);
		
		//SetIKPositionWeight 设置IK位置权重
		//SetIKRotationWeight 设置IK旋转权重
		//SetIKPosition       设置IK对应的位置
		//SetIKRotation       设置IK对应的角度
		//AvatarIKGoal        四肢末端IK枚举
		
        animator.SetIKPositionWeight(AvatarIKGoal.RightFoot, 1);
        animator.SetIKRotationWeight(AvatarIKGoal.RightFoot, 1);
        animator.SetIKPosition(AvatarIKGoal.RightFoot, pos2.position);
        animator.SetIKRotation(AvatarIKGoal.RightFoot, pos2.rotation);
    }
    
    private void OnAnimatorMove() { }
}

```