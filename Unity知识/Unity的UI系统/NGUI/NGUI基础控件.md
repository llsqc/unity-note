### Sprite精灵图片

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson3 : MonoBehaviour
{
    public UISprite sprite;

    void Start()
    {
        #region 知识点一 Sprite用来做啥
        //NGUI中所有中小尺寸图片显示都用Sprite显示
        //使用它来显示图集中的单个图片资源
        #endregion

        #region 知识点二 代码设置图片
        sprite.width = 200;
        sprite.height = 300;
        
        //1.改变为当前图集中选择的图片
        sprite.spriteName = "bk";

        //2.改变为其它图集中的图片
        //先加载图集
        NGUIAtlas atlas = Resources.Load<NGUIAtlas>("Atlas/login");
        sprite.atlas = atlas;
        
        //再设置图片
        sprite.spriteName = "ui_DL_anniuxiao_01";
        #endregion
    }
}

```

如何创建Sprite：
1. NGUI-Create-Sprite
2. 在Scene窗口中右键点击粉色框

![[Sprite参数1.bmp]]

![[Sprite参数2.bmp]]

![[Sprite参数3.bmp]]


### Label文本控件

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson4 : MonoBehaviour
{

    public UILabel label;
    void Start()
    {
        label.text = "123123123123";
    }
}
```

![[label参数1.bmp]]

![[label参数2.bmp]]

![[富文本.bmp]]

### Texture大图控件

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson5 : MonoBehaviour
{
    public UITexture tex;
    void Start()
    {
        //Sprite只能显示图集中图片 一般用于显示中小图片
        //如果使用大尺寸图片 没有必要打图集
        //直接使用Texture组件进行大图片显示

        Texture texture = Resources.Load<Texture>("BK");
        //改变图片
        if (texture != null)
            tex.mainTexture = texture;
    }
}

```

![[Texture参数.bmp]]