### 场景切换和退出游戏
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;

public class Lesson1 : MonoBehaviour
{
    void Update()
    {
        #region 知识点一 场景切换
        if( Input.GetKeyDown(KeyCode.Space) )
        {
            //切换到场景2
            //直接 写代码 切换场景 可能会报错
            //原因是没有把该场景加载到场景列表当中
            SceneManager.LoadScene("Test2");

            //用它不会报错 只会有警告 一样可以切换场景 
            //SceneManager
            //Application.LoadLevel("Test2");
        }
        #endregion

        #region 知识点二 退出游戏
        if( Input.GetKeyDown(KeyCode.Escape) )
        {
            //执行这句代码 就会退出游戏
            //但是 在编辑模式下没有作用
            //一定是发布游戏过后 才有用
            Application.Quit();
        }
        #endregion
    }
}

```

### 鼠标隐藏锁定
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson2 : MonoBehaviour
{
    public Texture2D tex;

    void Start()
    {
        #region 知识点一 隐藏鼠标
        Cursor.visible = true;
        #endregion

        #region 知识点二 锁定鼠标
        //None 就是 不锁定
        //Locked 锁定 鼠标会被限制在 屏幕的中心点 不仅会被锁定 还会被隐藏 可以通过ESC键 摆脱编辑模式下的锁定
        //Confined 限制在窗口范围内
        Cursor.lockState = CursorLockMode.Confined;

        #endregion

        #region 知识点三 设置鼠标图片
        //参数一：光标图片
        //参数二：偏移位置 相对图片左上角
        //参数三：平台支持的光标模式（硬件或软件）
        Cursor.SetCursor(tex, Vector2.zero, CursorMode.Auto);
        #endregion
    }
}

```

### 随机数和Unity自带委托
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Events;

public class Lesson3 : MonoBehaviour
{
    void Start()
    {
        #region 知识点一 随机数
        //Unity中的随机数
        //Unity当中 的Random类 此Random(Unity)非彼Random（C#）
        //使用随机数 int重载 规则是 左包含 右不包含
        //0~99之间的数
        int randomNum = Random.Range(0, 100);
        //float重载 规则是 左右都包含
        float randomNumF = Random.Range(1.1f, 99.9f);

        //C#中的随机数
        //System.Random r = new System.Random();
        //r.Next(0, 100);
        #endregion

        #region 知识点二 委托
        //C#的自带委托
        System.Action ac = () => {print("123");};

        System.Action<int, float> ac2 = (i, f) => { };

        System.Func<int> fun1 = () => {return 1;};

        System.Func<int,string> fun2 = (i) => {return "123";};
        //Unity的自带委托
        UnityAction uac = () = { };

        UnityAction<string> uac1 = (s) => { };
        #endregion
    }
}

```
