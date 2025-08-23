### Button 按钮控件
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class Lesson10 : MonoBehaviour
{
    void Start()
    {
        #region 知识点一 Button是什么
        //Button是按钮组件
        //是UGUI中用于处理玩家按钮相关交互的关键组件

        //默认创建的Button由2个对象组成
        //父对象——Button组件依附对象 同时挂载了一个Image组件 作为按钮背景图
        //子对象——按钮文本（可选）
        #endregion

        #region 知识点二 Button参数
        #endregion

        #region 知识点三 代码控制
        Button btn = this.GetComponent<Button>();
        btn.interactable = true;
        btn.transition = Selectable.Transition.None;

        Image img = this.GetComponent<Image>();
        #endregion

        #region 知识点四 监听点击事件的两种方式
        //点击事件 是 在按钮区域抬起按下一次 就算一次点击

        //1.拖脚本

        //2.代码添加
        btn.onClick.AddListener(ClickBtn2);
        btn.onClick.AddListener(() => {
            print("123123123");
        });

        btn.onClick.RemoveListener(ClickBtn2);
        btn.onClick.RemoveAllListeners();
        #endregion
    }

    public void ClickBtn()
    {
        print("按钮点击，通过拖代码的形式");
    }

    private void ClickBtn2()
    {
        print("按钮点击，通过拖代码的形式2");
    }
}
```

![[Button参数1.png]]
![[Button参数2.png]]

### Toggle 开关控件
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class Lesson11 : MonoBehaviour
{
    void Start()
    {
        #region 知识点一 Toggle是什么
        //Toggle是开关组件
        //是UGUI中用于处理玩家单选框多选框相关交互的关键组件
        
        //开关组件 默认是多选框
        //可以通过配合ToggleGroup组件制作为单选框
        
        //默认创建的Toggle由4个对象组成
        //父对象——Toggle组件依附
        //子对象——背景图（必备）、选中图（必备）、说明文字（可选）
        #endregion

        #region 知识点二 Toggle参数
        #endregion
        
        #region 知识点三 代码控制
        Toggle tog = this.GetComponent<Toggle>();
        tog.isOn = true;
        print(tog.isOn);
        
        ToggleGroup togGroup = this.GetComponent<ToggleGroup>();
        togGroup.allowSwitchOff = false;
        
        //可以遍历提供的迭代器 得到当前处于选中状态的 Toggle
        foreach (Toggle item in togGroup.ActiveToggles())
        {
            print(item.name + " " + item.isOn);
        }
        #endregion
        
        #region 知识点四 监听事件的两种方式
        //1.拖脚本
        //2.代码添加
        tog.onValueChanged.AddListener(ChangeValue2);
        tog.onValueChanged.AddListener((b) =>
        {
            print("代码监听 状态改变" + b);
        });
        #endregion
    }
    
    public void ChangValue(bool isOn)
    {
        print("状态改变" + isOn);
    }    
    
    private void ChangeValue2(bool v)
    {
        print("代码监听 状态改变" + v);
    }
}
```

![[Toggle参数.png]]


### InputField 文本输入控件
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class Lesson12 : MonoBehaviour
{
    void Start()
    {
        #region 知识点一 InputField是什么
        //InputField是输入字段组件
        //是UGUI中用于处理玩家文本输入相关交互的关键组件

        //默认创建的InputField由3个对象组成
        //父对象——InputField组件依附对象 以及 同时在其上挂载了一个Image作为背景图
        //子对象——文本显示组件（必备）、默认显示文本组件（必备）
        #endregion

        #region 知识点二 InputField参数

        #endregion

        #region 知识点三 代码控制
        InputField input = this.GetComponent<InputField>();
        print(input.text);
        input.text = "123123123123";
        #endregion

        #region 知识点四 监听事件的两种方式
        //1.拖脚本
        //2.代码添加
        input.onValueChanged.AddListener((str) =>
        {
            print("代码监听 改变" + str);
        });

        input.onEndEdit.AddListener((str) =>
        {
            print("代码监听 结束输入" + str);
        });
        #endregion
    }

    public void ChangeInput(string str)
    {
        print("改变的输入内容" + str);
    }

    public void EndInput(string str)
    {
        print("结束输入时内容" + str);
    }
}
```

![[InputField参数.png]]![[InputField参数2.png]]![[InputField参数3.png]]![[InputField参数4.png]]

### Slider 滑动条控件
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class Lesson13 : MonoBehaviour
{
    void Start()
    {
        #region 知识点一 Slider是什么
        //Slider是滑动条组件
        //是UGUI中用于处理滑动条相关交互的关键组件

        //默认创建的Slider由4组对象组成
        //父对象——Slider组件依附的对象
        //子对象——背景图、进度图、滑动块三组对象
        #endregion

        #region 知识点二 Slider参数
        #endregion

        #region 知识点三 代码控制
        Slider s = this.GetComponent<Slider>();
        print(s.value);

        #endregion

        #region 知识点四 监听事件的两种方式
        //1.拖脚本
        //2.代码添加
        s.onValueChanged.AddListener((v) =>
        {
            print("代码添加的监听" + v);
        });
        #endregion
    }

    public void ChangeValue(float v)
    {
        print(v);
    }
}
```

![[Slider参数.png]]

### ScrollBar 滚动条控件
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class Lesson14 : MonoBehaviour
{
    void Start()
    {
        #region 知识点一 Scrollbar是什么
        //Scrollbar是滚动条组件
        //是UGUI中用于处理滚动条相关交互的关键组件

        //默认创建的Scrollbar由2组对象组成
        //父对象——Scrollbar组件依附的对象
        //子对象——滚动块对象

        //一般情况下我们不会单独使用滚动条 
        //都是配合ScrollView滚动视图来使用
        #endregion

        #region 知识点二 Scrollbar参数

        #endregion

        #region 知识点三 代码控制
        Scrollbar sb = this.GetComponent<Scrollbar>();
        print(sb.value);
        print(sb.size);
        #endregion

        #region 知识点四 监听事件的两种方式
        //1.拖脚本
        //2.代码添加
        sb.onValueChanged.AddListener((v) => {
            print("代码监听的函数" + v);
        });
        #endregion
    }

    public void ChangeValue(float v)
    {
        print(v);
    }
}
```

![[Scrollbar参数.png]]

### ScrollView 滚动视图控件
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class Lesson15 : MonoBehaviour
{
    void Start()
    {
        #region 知识点一 ScrollRect是什么
        //ScrollRect是滚动视图组件
        //是UGUI中用于处理滚动视图相关交互的关键组件

        //默认创建的ScrollRect由4组对象组成
        //父对象——ScrollRect组件依附的对象 还有一个Image组件 最为背景图
        //子对象
        //Viewport控制滚动视图可视范围和内容显示
        //Scrollbar Horizontal 水平滚动条
        //Scrollbar Vertical 垂直滚动条

        #endregion

        #region 知识点二 ScrollRect参数

        #endregion

        #region 知识点三 代码控制
        ScrollRect sr = this.GetComponent<ScrollRect>();
        //改变内容的大小 具体可以拖动多少 都是根据它的尺寸来的
        //sr.content.sizeDelta = new Vector2(200, 200);

        sr.normalizedPosition = new Vector2(0, 0.5f);
        #endregion

        #region 知识点四 监听事件的两种方式
        //1.拖脚本
        //2.代码添加

        sr.onValueChanged.AddListener((vec) =>
        {
            print(vec);
        });
        #endregion
    }

    public void ChangeValue(Vector2 v)
    {
        print(v);
    }
}
```

![[ScrollView参数.png]]

### Dropdown 下拉列表控件
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class Lesson16 : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 DropDown是什么
        //DropDown是下拉列表（下拉选单）组件
        //是UGUI中用于处理下拉列表相关交互的关键组件

        //默认创建的DropDown由4组对象组成
        //父对象
        //DropDown组件依附的对象 还有一个Image组件 作为背景图

        //子对象
        //Label是当前选项描述
        //Arrow右侧小箭头
        //Template下拉列表选单

        #endregion

        #region 知识点二 DropDown参数

        #endregion

        #region 知识点三 代码控制
        Dropdown dd = this.GetComponent<Dropdown>();

        print(dd.value);

        print(dd.options[dd.value].text);

        dd.options.Add(new Dropdown.OptionData("123123123"));
        #endregion

        #region 知识点四 监听事件的两种方式
        //1.拖脚本
        //2.代码添加
        dd.onValueChanged.AddListener((index) => {

            print(index);
        });
        #endregion
    }

    public void ChangeValue(int value)
    {
        print(value);
    }
}

```

![[Dropdown参数.png]]

