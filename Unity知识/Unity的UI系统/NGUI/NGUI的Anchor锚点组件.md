```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson13 : MonoBehaviour
{
    void Start()
    {
        #region 知识点一 Anchor是什么
        //是用于9宫格布局的锚点
        //它有两个关键知识点
        //1.老版本——锚点组件——用于控制对象对齐方式
        //2.新版本——3大基础控件自带 锚点内容——用于控制对象相对父对象布局
        #endregion

        #region 知识点二 老版本——锚点组件
        //主要用于设置面板相对屏幕的9宫格位置
        //用于控制对象对齐方式
        #endregion

        #region 知识点三 新版本——基础控件自带锚点信息
        //用于控制对象相对父对象布局
        #endregion
    }
}

```

![[Anchor参数.bmp]]