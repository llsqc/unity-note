### 文本和按钮

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson2 : MonoBehaviour
{
    public Texture tex;
    public Rect rect;

    public Rect rect1;

    public GUIContent content;

    public GUIStyle style;

    public Rect btnRect;
    public GUIContent btnContent;
    public GUIStyle btnStyle;

    private void OnGUI()
    {
        #region 知识点一 GUI 控件绘制的共同点
        //1.他们都是GUI公共类中提供的静态函数 直接调用即可
        //2.他们的参数都大同小异
        //  位置参数：Rect参数  x y位置 w h尺寸
        //  显示文本：string参数
        //  图片信息：Texture参数
        //  综合信息：GUIContent参数
        //  自定义样式：GUIStyle参数
        //3.每一种控件都有多种重载，都是各个参数的排列组合
        //  必备的参数内容 是 位置信息和显示信息
        #endregion

        #region 知识点二 文本控件
        
        //基本使用
        GUI.Label(new Rect(0, 0, 100, 20), "唐老狮欢迎你", style);
        GUI.Label(rect, tex);
        
        //综合使用
        GUI.Label(rect1, content);
        
        //可以获取当前鼠标或者键盘选中的GUI控件 对应的 tooltip信息
        Debug.Log(GUI.tooltip);
        
        //自定义样式
        #endregion

        #region 知识点三 按钮控件
        //基本使用
        //综合使用
        //自定义样式
        //在按钮范围内 按下鼠标再抬起鼠标 才算一次点击 才会返回true
        if (GUI.Button(btnRect, btnContent, btnStyle))
        {
            //处理我们按钮点击的逻辑
            Debug.Log("按钮被点击");
        }

        //只要在长按按钮范围内 按下鼠标 就会一直返回true
        if( GUI.RepeatButton(btnRect, btnContent) )
        {
            Debug.Log("长按按钮被点击");
        }
        #endregion
    }
}

```

### 多选框和单选框

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson3 : MonoBehaviour
{
    private bool isSel;
    private bool isSel2;

    public GUIStyle style;

    private int nowSelIndex = 1;
    private void OnGUI()
    {
        #region 知识点一 多选框
        #region 普通样式
        isSel = GUI.Toggle(new Rect(0, 0, 100, 30), isSel, "效果开关");
        #endregion

        #region 自定义样式 显示问题
        //修改固定宽高 fixedWidth和fixedHeight
        //修改从GUIStyle边缘到内容起始处的空间 padding

        isSel2 = GUI.Toggle(new Rect(0, 40, 100, 30), isSel2, "音效开关", style);
        #endregion
        #endregion

        #region 知识点二 单选框
        //单选框是基于 多选框的实现
        //关键：通过一个int标识来决定是否选中 
        if(GUI.Toggle(new Rect(0, 100, 100, 30), nowSelIndex == 1, "选项一"))
        {
            nowSelIndex = 1;
        }
        if(GUI.Toggle(new Rect(0, 140, 100, 30), nowSelIndex == 2, "选项二"))
        {
            nowSelIndex = 2;
        }
        if(GUI.Toggle(new Rect(0, 180, 100, 30), nowSelIndex == 3, "选项三"))
        {
            nowSelIndex = 3;
        }
        #endregion
    }
}

```

### 输入框和拖动条

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson4 : MonoBehaviour
{
    private string inputStr = "";
    private string inputPW = "";
    private float nowValue = 0.5f;
    
    private void OnGUI()
    {
        #region 知识点一 输入框

        #region 普通输入
        //输入框 重要参数 一个是显示内容 string
        //一个是 最大输入字符串的长度
        inputStr = GUI.TextField(new Rect(0, 0, 100, 30), inputStr, 5);
        #endregion

        #region 密码输入
        inputPW = GUI.PasswordField(new Rect(0, 50, 100, 30), inputPW, '★');
        #endregion

        #endregion

        #region 知识点二 拖动条

        #region 水平拖动条
        // 当前的值
        // 最小值 left
        // 最大值 right
        nowValue = GUI.HorizontalSlider(new Rect(0, 100, 100, 50), nowValue, 0, 1);
        #endregion

        #region 竖直拖动条
        nowValue = GUI.VerticalSlider(new Rect(0, 150, 50, 100), nowValue, 0, 1);
        #endregion

        #endregion
    }
}

```

### 图片和框的绘制

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson5 : MonoBehaviour
{
    public Rect texPos;
    public Texture tex;
    public ScaleMode mode = ScaleMode.StretchToFill;
    public bool alpha = true;
    public float wh = 0;

    private void OnGUI()
    {
        #region 知识点一 图片绘制
        // ScaleMode
        // ScaleAndCrop:也会通过宽高比来计算图片 但是 会进行裁剪
        // ScaleToFit：会自动根据宽高比进行计算 不会拉变形 会一直保持图片完全显示的状态
        // StretchToFill:始终填充满你传入的 Rect范围

        // alpha 是用来控制图片是否开启透明通道的

        // imageAspect ： 自定义宽高比，如果不填默认为0，就会使用图片原始宽高  
        GUI.DrawTexture(texPos, tex, mode, alpha, wh);
        #endregion

        #region 知识点二 框绘制
        GUI.Box(texPos, "");
        #endregion
    }
}

```