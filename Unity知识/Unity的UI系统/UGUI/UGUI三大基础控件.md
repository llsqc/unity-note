### Image 图像控件
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class Lesson7 : MonoBehaviour
{
    void Start()
    {
        #region 知识点一 Image是什么？
        //Image是图像组件
        //是UGUI中用于显示精灵图片的关键组件
        //除了背景图等大图，一般都使用Image来显示UI中的图片元素
        #endregion
        
        #region 知识点二 Image参数
        #endregion
        
        #region 知识点三 代码控制
        Image img = this.GetComponent<Image>();
        img.sprite = Resources.Load<Sprite>("ui_TY_fanhui_01");
        
        (transform as RectTransform).sizeDelta = new Vector2(200, 200);
        img.raycastTarget = false;
        
        img.color = Color.red;
        #endregion
    }
}
```

![[Image参数.png]]


### Text 文本控件
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class Lesson8 : MonoBehaviour
{
    void Start()
    {
        #region 知识点一 Text是什么
        //Text是文本组件
        //是UGUI中用于显示文本的关键组件
        #endregion
        
        #region 知识点二 Text参数相关
        #endregion
        
        #region 知识点三 富文本
        #endregion
        
        #region 知识点四 边缘线和阴影
        //边缘线组件 outline
        //阴影组件 Shadow
        #endregion
        
        #region 知识点五 代码控制
        Text txt = this.GetComponent<Text>();
        txt.text = "This is a new text";
        #endregion
    }
}
```

![[Text参数.png]]

![[Text富文本.png]]

### RawImage 原始图像控件知识点
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class Lesson9 : MonoBehaviour
{
    void Start()
    {
        #region 知识点一 RawImage是什么
        //RawImage是原始图像组件
        //是UGUI中用于显示任何纹理图片的关键组件
        //它和Image的区别是 一般RawImage用于显示大图(背景图，不需要打入图集的图片，网络下载的图等等)
        #endregion
        
        #region 知识点二 RawIamge参数
        #endregion
        
        #region 知识点三 代码控制
        RawImage raw = this.GetComponent<RawImage>();
        raw.texture = Resources.Load<Texture>("ui_TY_lvseshuzi_08");
        raw.uvRect = new Rect(0, 0, 1, 1);
        #endregion
    }
}
```

![[RawImage参数.png]]
