
**Unity自带的图集功能：**

**在工程设置面板中打开功能**
**Edit**——>**Project Setting**——>**Editor**——>**Sprite Packer**

**Disabled：默认设置，不会打包图集**
**Enabled For Builds（Legacy Sprite Packer）：Unity仅在构建时打包图集，在编辑模式下不会打包图集**
**Always Enabled（Legacy Sprite Packer）：Unity在构建时打包图集，在编辑模式下运行前会打包图集**

**Legacy Sprite Packer：传统打包模式 相对下面两种模式来说 多了一个设置图片之间的间隔距离**
**Padding Power：选择打包算法在计算打包的精灵之间以及精灵与生成的图集边缘之间的间隔距离，这里的数字 代表2的n次方**

**Enabled For Build：Unity进在构建时打包图集，在编辑器模式下不会打包**
**Always Enabled：Unity在构建时打包图集，在编辑模式下运行前会打包图集**


```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.U2D;

public class Lesson17 : MonoBehaviour
{
    void Start()
    {
        #region 知识点 代码加载
        //加载图集 注意：需要引用命名空间
        SpriteAtlas sa = Resources.Load<SpriteAtlas>("MyAlas");
        //从图集中加载指定名字的小图
        sa.GetSprite("bk");
        #endregion
    }
}
```