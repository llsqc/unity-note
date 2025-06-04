**什么是Tilemap**

Tilemap一般称之为 瓦片地图或者平铺地图
是Unity2017中新增的功能
主要用于快速编辑2D游戏中的场景
通过复用资源的形式提升地图多样性
工作原理就是用一张张的小图排列组合为一张大地图

**它和SpriteShape的异同**
共同点：
他们都是用于制作2D游戏的场景或地图的
不同点
1. SpriteShape可以让地形有弧度,TileMap不行
2. TileMap可以快捷制作有伪“Z”轴的地图，SpriteShape不行

Tilemap的最小单位——"瓦片"

首先导入学习用资源

方法一：
Assets——>Create——>Tile

方法二：
在Tile Palette瓦片调色板窗口创建
1.首先新建一个瓦片地图编辑文件
2.将资源拖入到窗口中选择要保存的路径

![[瓦片资源参数 相关.png]]

### 瓦片调色板窗口

编辑瓦片地图
方法一：通过瓦片调色板文件创建
方法二：直接在场景中进行创建

矩形瓦片地图用于做横版游戏地图
六边形瓦片地图用于做策略游戏地图
等距瓦片地图用于做有"Z"轴的2D游戏

注意：
在编辑等距瓦片地图时
1.需要修改工程的自定义轴排序 以Y轴决定渲染顺序
2.如果地图存在前后关系需要修改TileRenderer的渲染模式
3.可以通过Z轴偏移来控制绘制单个瓦片时的高度
4.精灵纹理的中心点会影响最终的显示效果

![[瓦片调色板窗口使用 参数相关.xmind]]

### 瓦片地图关键脚本和碰撞器

参数设置
![[瓦片地图关键脚本和碰撞器.xmind]]

瓦片地图碰撞器
为挂载TilemapRenerer脚本的对象添加Tilemap Collider2D脚本
会自动添加碰撞器
注意：想要生成碰撞器的瓦片Collider Type类型要进行设置

实践：完成横板地图和伪z轴地图，处理相关问题

### 瓦片地图拓展包

[TileMap拓展包](https://github.com/Unity-Technologies/2d-extras)
拓展包为Tilemap添加新的瓦片类型和笔刷类型 帮助我们更加方便的编辑2D场景

#### 新增瓦片类型

一 规则瓦片 RuleTile
定义不同方向是否存在连接图片的规则
让我们更加快捷的进行地图编辑

二 动画瓦片 AnimatedTile
可以指定序列帧
产生可以播放序列帧动画的瓦片

三 管道瓦片 PipelineTile
根据自己相邻瓦片的数量更换显示的图片

四 随机瓦片 RandomTile
根据你设置的图片，随机从中选一个进行绘制

五 地形瓦片 TerrainTile
有点类似规则瓦片，只不过地形瓦片是帮助你定好的规则

六 权重随机瓦片 WeightedRandomTile
可以不平均随机选择图片的瓦片

七 (高级)规则覆盖瓦片 (Advanced)Rule Override Tile
在规则瓦片的基础上 改变图片或者指定启用的规则

![[官方拓展包—新增瓦片类型.xmind]]

#### 新增笔刷类型

**新建自定义笔刷**
1. 预设体笔刷——用于快捷刷出想要创建的精灵
2. 预设体随机笔刷——用于快捷随机刷出想要创建的精灵
3. 随机笔刷——可以指定瓦片进行关联，随机刷出对应瓦片

**拓展笔刷**
1. Coordinate Brush 坐标笔刷 —— 可以实时看到格子坐标
2. GameObject Brush 游戏对象笔刷 —— 可以在场景中选择和擦除游戏对象仅限于选定的游戏对象的子级
3. Group Brush 组合笔刷 —— 可以设置参数 当点击一个瓦片样式时 会自动取出一个范围内的瓦片
4. Line Brush 线性笔刷 —— 决定起点和终点画一条线出来
5. Random Brush 随机笔刷 —— 和之前的自定义随机画笔类似，可以随机画出瓦片
6. Tint Brush 着色笔刷 —— 可以给瓦片着色 瓦片的颜色锁要开启（Inspector窗口切换Debug模式 修改Flags）
7. Tint Brush(Smooth) 光滑着色笔刷 —— 可以给瓦片进行渐变着色，需要按要求改变材质

### 代码控制

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Tilemaps;

public class Lesson28 : MonoBehaviour
{
    //瓦片地图信息 可以通过它得到瓦片格子
    public Tilemap map;
    //格子位置相关控制 可以通过它 进行坐标转换
    public Grid grid;
    //瓦片资源基类通过它可以得到瓦片资源
    public TileBase tileBase;

    void Start()
    {
        #region 知识点一 获取Tilemap和TileBase和Grid
        //Tilemap组件：用于管理瓦片地图
        //TileBase组件：瓦片资源对象基类
        //Grid组件：用于坐标转换

        //使用他们需要引用命名空间
        #endregion

        #region 知识点二 重要API
        //1.清空瓦片地图
        //map.ClearAllTiles();

        //2.获取指定坐标格子
        TileBase tmp = map.GetTile(Vector3Int.zero);
        print(tmp);

        //3.设置删除瓦片
        map.SetTile(new Vector3Int(0, 2, 0), tileBase);
        map.SetTile(new Vector3Int(1, 0, 0), null);

        //map.SetTiles()

        //4.替换瓦片
        map.SwapTile(tmp, tileBase);

        //5.世界坐标转格子坐标

        //  屏幕坐标转世界坐标
        //  世界坐标转格子坐标
        //传入的参数是世界坐标
        grid.WorldToCell()
        #endregion
    }
}

```