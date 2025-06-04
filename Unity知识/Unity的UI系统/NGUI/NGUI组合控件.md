### Button按钮组件

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson6 : MonoBehaviour
{
    public UIButton btn;

    void Start()
    {
        #region 知识点一 所有组合控件的共同特点
        //1.在3个基础组件对象上添加对应组件
        //2.如果希望响应点击等事件 需要添加碰撞器
        #endregion

        #region 知识点三 制作Button
        //1.一个Sprite（需要文字再加一个Label子对象）
        //2.为Sprite添加Button脚本
        //3.添加碰撞器
        #endregion

        #region 知识点五 监听事件的两种方式
        //1.拖脚本
        //2.代码获取按钮对象监听
        btn.onClick.Add(new EventDelegate(ClickDo2));

        btn.onClick.Add(new EventDelegate(() => {
            print("Lambda表达式添加的点击事件处理");
        }));
        #endregion
    }

    public void ClickDoSomthing()
    {
        print("按钮点击");
    }
    public void ClickDo2()
    {
        print("按钮点击2");
    }
}

```

![[按钮参数相关.bmp]]

### Toggle单选多选框组件

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson7 : MonoBehaviour
{
    public UIToggle tog1;
    public UIToggle tog2;
    public UIToggle tog3;

    void Start()
    {
        #region 知识点一 Toggle用来干啥
        //单选框 多选框都可以使用它来制作
        #endregion

        #region 知识点二 制作Toggle
        //1.2个Sprite 1父1子
        //2.为父对象添加Toggle脚本
        //3.添加碰撞器
        #endregion

        #region 知识点三 参数相关

        #endregion

        #region 知识点四 监听事件的两种方式
        //1.拖代码
        //2.代码进行监听添加

        tog1.onChange.Add(new EventDelegate(Change2));
        tog2.onChange.Add(new EventDelegate(Change2));
        tog3.onChange.Add(new EventDelegate(Change2));
        #endregion
    }

    private void Change2()
    {
        print("代码监听");
    }
   

    public void Change()
    {
        print("Toggle变化执行的内容");

        if( tog1.value )
        {
            print("tog1选中");
        }
        else if (tog2.value)
        {
            print("tog2选中");
        }
        else if (tog3.value)
        {
            print("tog3选中");
        }
    }
}

```

![[Toggle参数.bmp]]

### Input输入框组件

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson8 : MonoBehaviour
{
    public UIInput input;

    void Start()
    {
        #region 知识点一 Input是啥
        //输入框
        //可以用来制作账号密码聊天输入框
        #endregion

        #region 知识点二 制作Input
        //1.1个Sprite做背景 1个Label显示文字
        //2.为Sprint添加Input脚本
        //3.添加碰撞器
        #endregion

        #region 知识点三 参数相关

        #endregion

        #region 知识点四 监听事件的两种方式
        //1.拖曳脚本
        //2.通过代码关联

        input.onSubmit.Add(new EventDelegate(() =>
        {
            print("完成输入 通关代码添加的监听函数");
        }));

        input.onChange.Add(new EventDelegate(() =>
        {
            print("输入变化 通关代码添加的监听函数");
        }));
        #endregion
    }

    public void OnSubmit()
    {
        print("输入完成" + input.value);
    }

    public void OnChange()
    {
        print("输入变化" + input.value);
    }
}
```

![[输入框参数1.bmp]]

![[输入框参数2.bmp]]

### PopupList下拉列表

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson9 : MonoBehaviour
{
    public UIPopupList list;

    void Start()
    {
        #region 知识点一 PopupList是啥？
        //下拉列表 
        #endregion

        #region 知识点二 制作Popuplist
        //1.一个sprite做背景 一个lable做显示内容
        //2.添加PopupList脚本
        //3.添加碰撞器
        //4.关联lable做信息更新，选择Label中的SetCurrentSelection函数
        #endregion

        #region 知识点三 参数相关

        #endregion

        #region 知识点四 监听事件的两种方式
        //1.拖曳代码
        //2.代码关联
        list.items.Add("新加 选项4");

        list.onChange.Add(new EventDelegate(() => {

            print("代码添加的监听" + list.value);
        }));
        #endregion
    }

    public void OnChange()
    {
        print("选项变化" + list.value);
    }
}
```

![[PopupList参数1.bmp]]

![[PopupList参数2.bmp]]

![[PopupList参数3.bmp]]

### Slider滑动条控件

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson10 : MonoBehaviour
{
    public UISlider slider;

    void Start()
    {
        #region 知识点一 Slider是啥？
        //滑动条控件
        //主要用于设置音乐音效大小等
        #endregion

        #region 知识点二 制作Slider
        //1.3个sprite 1个做根对象为背景  2个子对象 1个进度 1个滑动块 
        //2.设置层级
        //3.为根背景添加Slider脚本
        //4.添加碰撞器（父对象或者滑块）
        //5.关联3个对象
        #endregion

        #region 知识点三 参数相关

        #endregion

        #region 知识点四 监听事件的两种方式
        //1.拖曳脚本关联
        //2.通过代码关联
        slider.onChange.Add(new EventDelegate(() => {

            print("通过代码监听" + slider.value);
        }));

        slider.onDragFinished += () => {
            print("拖曳结束" + slider.value);
        };
        #endregion
    }

    public void OnChange()
    {
        print("值变化" + slider.value);
    }
}

```

![[Slider参数相关.bmp]]

### ScrollBar滚动条

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson11 : MonoBehaviour
{

    void Start()
    {
        #region 知识点一 ScrollBar和ProgressBar用来干啥
        //1.ScrollBar滚动条一般不单独使用 都是配合滚动视图使用 类似VS右侧的滚动条
        //2.ProgressBar进度条 一般不咋使用   一般直接用Sprite的Filed填充模式即可

        //他们的参数和之前的知识很类似
        //所以这两个知识点不是重点 了解如何制作他们即可
        #endregion

        #region 知识点二 制作Scrollbar
        //1.两个Sprite 1个背景 1个滚动条
        //2.背景父对象添加脚本
        //3.添加碰撞器
        //4.关联对象
        #endregion

        #region 知识点三 制作ProgressBar
        //1.两个Sprite 1个背景 1个进度条
        //2.背景父对象添加脚本
        //3.关联对象
        #endregion
    }
}

```

### ScrollView滚动视图

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson12 : MonoBehaviour
{
    void Start()
    {
        #region 知识点一 ScrollView来干啥
        //滚动视图
        //我们现在用于编程的VS代码窗口就是典型的滚动视图
        //游戏中主要用于 背包、商店、排行榜等等功能
        #endregion
        
        #region 知识点二 制作ScrollView
        //1.直接工具栏创建即可 NGUI——Create——ScrollView
        //2.若需要ScrollBar 自行添加水平和竖直
        //3.添加子对象 为子对象添加Drag Scroll View和碰撞器
        #endregion
        
        #region 知识点三 参数相关
        #endregion
        
        #region 知识点四 自动对齐脚本Grid 参数相关
        #endregion
    }
}

```

![[ScrollView参数1.bmp]]

![[ScrollView参数2.bmp]]

![[ScrollView的Grid参数.bmp]]
