### 工具栏和选择网络
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson6 : MonoBehaviour
{
    private int toolbarIndex = 0;
    private string[] toolbarInfos = new string[] { "强化", "进阶", "幻化" };

    private int selGridIndex = 0;

    private void OnGUI()
    {
        #region 知识点一 工具栏
        toolbarIndex = GUI.Toolbar(new Rect(0, 0, 200, 30), toolbarIndex, toolbarInfos);
        //工具栏可以帮助我们根据不同的返回索引 来处理不同的逻辑
        switch (toolbarIndex)
        {
            case 0:
                break;
            case 1:
                break;
            case 2:
                break;
        }
        #endregion

        #region 知识点二 选择网格
        //相对toolbar多了一个参数 xCount 代表 水平方向最多显示的按钮数量
        selGridIndex = GUI.SelectionGrid(new Rect(0, 50, 200, 60), selGridIndex, toolbarInfos, 1);
        #endregion
    }
}

```

### 滚动视图和分组
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson7 : MonoBehaviour
{
    public Rect groupPos;


    public Rect scPos;
    public Rect showPos;

    private Vector2 nowPos;

    private string[] strs = new string[] { "123", "234", "222", "111" };
    private void OnGUI()
    {
        #region 知识点一 分组
        // 用于批量控制控件位置 
        // 可以理解为 包裹着的控件加了一个父对象 
        // 可以通过控制分组来控制包裹控件的位置
        GUI.BeginGroup(groupPos);

        GUI.Button(new Rect(0, 0, 100, 50), "测试按钮");
        GUI.Label(new Rect(0, 60, 100, 20), "Label信息");

        GUI.EndGroup();

        #endregion

        
        #region 知识点二 滚动列表
        nowPos = GUI.BeginScrollView(scPos, nowPos, showPos);

        GUI.Toolbar(new Rect(0, 0, 300, 50), 0, strs);
        GUI.Toolbar(new Rect(0, 60, 300, 50), 0, strs);
        GUI.Toolbar(new Rect(0, 120, 300, 50), 0, strs);
        GUI.Toolbar(new Rect(0, 180, 300, 50), 0, strs);

        GUI.EndScrollView();
        #endregion
    }
}

```

### 窗口
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson8 : MonoBehaviour
{

    private Rect dragWinPos = new Rect(400, 400, 200, 150);
    private void OnGUI()
    {
        #region 知识点一 窗口
        //第一个参数 id 是窗口的唯一ID 不要和别的窗口重复
        //委托参数 是用于 绘制窗口用的函数 传入即可
        GUI.Window(1, new Rect(100, 100, 200, 150), DrawWindow, "测试窗口");
        //id对于我们来说 有一个重要作用 除了区分不同窗口 还可以在一个函数中去处理多个窗口的逻辑
        //通过id去区分他们
        GUI.Window(2, new Rect(100, 350, 200, 150), DrawWindow, "测试窗口2");
        #endregion

        #region 知识点二 模态窗口
        //模态窗口 可以让该其它控件不在有用
        //你可以理解该窗口在最上层 其它按钮都点击不到了
        //只能点击该窗口上控件

        GUI.ModalWindow(3, new Rect(300, 100, 200, 150), DrawWindow, "模态窗口");
        #endregion

        #region 知识点三 拖动窗口
        //位置赋值只是前提
        dragWinPos = GUI.Window(4, dragWinPos, DrawWindow, "拖动窗口");
        #endregion
    }

    private void DrawWindow(int id)
    {
        switch (id)
        {
            case 1:
                GUI.Button(new Rect(0, 30, 30, 20), "1");
                break;
            case 2:
                GUI.Button(new Rect(0, 30, 30, 20), "2");
                break;
            case 3:
                GUI.Button(new Rect(0, 30, 30, 20), "3");
                break;
            case 4:
                //该API 写在窗口函数中调用 可以让窗口被拖动
                //传入Rect参数的重载 作用 
                //是决定窗口中哪一部分位置 可以被拖动
                //默认不填 就是无参重载 默认窗口的所有位置都能被拖动
                GUI.DragWindow(new Rect(0,0,1000,20));
                break;
        }
        
    }
}

```

### 自定义整体样式
#### GUISkin
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson9 : MonoBehaviour
{
    public GUIStyle style;

    public GUISkin skin;
    private void OnGUI()
    {
        #region 知识点一 全局颜色
        //全局的着色颜色 影响背景和文本颜色
        GUI.color = Color.red;

        //文本着色颜色 会和 全局颜色相乘
        GUI.contentColor = Color.yellow;
        GUI.Button(new Rect(0, 0, 100, 30), "测试按钮");
        
        //背景元素着色颜色 会和 全局颜色相乘
        GUI.backgroundColor = Color.red;
        GUI.Label(new Rect(0, 50, 100, 30), "测试按钮");
        GUI.color = Color.white;
        GUI.Button(new Rect(0, 100, 100, 30), "测试按钮", style);

        #endregion

        #region 知识点二 整体皮肤样式
        GUI.skin = skin;
        //虽然设置了皮肤 但是绘制时 如果使用GUIStyle参数 皮肤就没有
        GUI.Button(new Rect(0, 0, 100, 30), "测试按钮");

        GUI.skin = null;
        GUI.Button(new Rect(0, 50, 100, 30), "测试按钮2");

        //它可以帮助我们整套的设置 自定义样式 
        //相对单个控件设置Style要方便一些
        #endregion
    }
}

```

#### GUILayout
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson10 : MonoBehaviour
{
    private void OnGUI()
    {
        #region 知识点一 GUILayout 自动布局
        //主要用于进行编辑器开发 如果用它来做游戏UI不太合适
        GUI.BeginGroup(new Rect(100, 100, 200, 300));
        GUILayout.BeginVertical();

        GUILayout.Button("123", GUILayout.Width(200));
        GUILayout.Button("245666656565");
        GUILayout.Button("235", GUILayout.ExpandWidth(false));

        GUILayout.EndVertical();
        GUI.EndGroup();
        #endregion

        #region 知识点二 GUILayoutOption 布局选项
        
        //控件的固定宽高
        GUILayout.Width(300);
        GUILayout.Height(200);
        
        //允许控件的最小宽高
        GUILayout.MinWidth(50);
        GUILayout.MinHeight(50);
        
        //允许控件的最大宽高
        GUILayout.MaxWidth(100);
        GUILayout.MaxHeight(100);
        
        //允许或禁止水平拓展
        GUILayout.ExpandWidth(true);//允许
        GUILayout.ExpandHeight(false);//禁止
        
        GUILayout.ExpandHeight(true);//允许
        GUILayout.ExpandHeight(false);//禁止
        
        #endregion
    }
}

```