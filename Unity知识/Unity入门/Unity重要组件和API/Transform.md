#### Transform——位置和位移

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson6 : MonoBehaviour
{
    void Start()
    {
        #region Transform主要用来干嘛？
        //游戏对象（GameObject）位移、旋转、缩放、父子关系、坐标转换等相关操作都由它处理
        //它是Unity提供的极其重要的类
        #endregion

        #region 知识点一 必备知识点 Vector3基础
        //Vector3主要是用来表示三维坐标系中的 一个点 或者一个向量
        //申明
        Vector3 v = new Vector3();
        v.x = 10;
        v.y = 10;
        v.z = 10;
        //只传xy 默认z是0
        Vector3 v2 = new Vector3(10, 10);
        //一步到位
        Vector3 v3 = new Vector3(10, 10, 10);

        Vector3 v4;
        v4.x = 10;
        v4.y = 10;
        v4.z = 10;

        //Vector的基本计算
        // + - * /
        Vector3 v1 = new Vector3(1, 1, 1);
        Vector3 v12 = new Vector3(2, 2, 2);
        // 对应用x+或者-x.....
        print(v1 + v12);
        print(v1 - v12);

        print(v1 * 10);
        print(v12 / 2);

        //常用
        print(Vector3.zero);//000
        print(Vector3.right);//100
        print(Vector3.left);//-100
        print(Vector3.forward);//001
        print(Vector3.back);//00-1
        print(Vector3.up);//010
        print(Vector3.down);//0-10

        //常用的一个方法 
        //计算两个点之间的距离的方法
        print(Vector3.Distance(v1, v12));
        #endregion

        #region 知识点二 位置
        //相对世界坐标系
        //this.gameObject.transform
        //通过position得到的位置 是相对于 世界坐标系的 原点的位置
        //可能和面板上显示的 是不一样的
        //因为如果对象有父子关系 并且父对象位置 不在原点 那么 和面板上肯定就是不一样的
        print(this.transform.position);

        //相对父对象
        //这两个坐标 对于我们来说 很重要 如果你想以面板坐标为准来进行位置设置
        //那一定是通过localPosition来进行设置的
        print(this.transform.localPosition);

        //他们两个 可能出现是一样的情况
        //1.父对象的坐标 就是世界坐标系原点0,0,0
        //2.对象没有父对象 

        //注意：位置的赋值不能直接改变x，y，z 只能整体改变
        //不能单独改 x y z某一个值
        //this.transform.position.x = 10;
        this.transform.position = new Vector3(10, 10, 10);
        this.transform.localPosition = Vector3.up * 10;
        
        //如果只想改一个值x y和z要保持原有坐标一致
        //1.直接赋值
        this.transform.position = new Vector3(19, this.transform.position.y, this.transform.position.z);
        //2.先取出来 再赋值
        //虽然不能直接改 transform的 xyz 但是 Vector3是可以直接改 xyz的
        //所以可以先取出来改Vector3 再重新赋值
        Vector3 vPos = this.transform.localPosition;
        vPos.x = 10;
        this.transform.localPosition = vPos;

        //如果你想得到对象当前的 一个朝向 
        //那么就是通过 transform 出来的 
        //对象当前的各朝向
        //对象当前的面朝向
        print(this.transform.forward);
        //对象当前的头顶朝向
        print(this.transform.up);
        //对象当前的右手边
        print(this.transform.right);

        #endregion
    }

    void Update()
    {
        #region 知识点三 位移
        //理解坐标系下的位移计算公式
        //路程 = 方向 * 速度 * 时间

        //方式一 自己计算
        //想要变化的 就是 position

        //用当前的位置 + 我要动多长距离  得出最终所在的位置
        this.transform.position = this.transform.position + this.transform.up * 1 * Time.deltaTime;

        //因为我用的是 this.transform.forward 所以它始终会朝向相对于自己的面朝向去动
        this.transform.position += this.transform.forward * 1 * Time.deltaTime;

        //方向非常重要 因为 它决定了你的前进方向
        this.transform.position += Vector3.forward * 1 * Time.deltaTime;

        //方式二 API
        //参数一：表示位移多少  路程 = 方向 * 速度 * 时间
        //参数二：表示 相对坐标系   默认 该参数 是相对于自己坐标系的

        //1相对于世界坐标系的 Z轴 动  始终是朝 世界坐标系 的 Z轴正方向移动
        this.transform.Translate(Vector3.forward * 1 * Time.deltaTime, Space.World);

        //2相对于世界坐标的 自己的面朝向去动   始终朝自己的面朝向移动
        this.transform.Translate(this.transform.forward * 1 * Time.deltaTime, Space.World);

        //3相对于自己的坐标系 下的 自己的面朝向向量移动 （一定不会这样让物体移动） XXXXXXX
        this.transform.Translate(this.transform.forward * 1 * Time.deltaTime, Space.Self);

        //4相对于自己的坐标系 下的 Z轴正方向移动  始终朝自己的面朝向移动
        this.transform.Translate(Vector3.forward * 1 * Time.deltaTime, Space.Self);

        //注意：一般使用API来进行位移
        #endregion
    }

    //总结
    //Vector3
    //如何申明 提供的 常用静态属性 和一个 计算距离的方法 
    //位置
    //相对于世界坐标系 和 相对于父对象 这两个坐标的区别
    //不能够 单独修改 xyz  只能一起统一改 
    //位移
    //自己如何计算来进行位移
    //API是哪个方法 来进行位移 使用时 要注意
}

```

#### Transform——角度和旋转

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson7 : MonoBehaviour
{
    void Start()
    {
        #region 知识点一 角度相关
        //相对世界坐标角度
        print(this.transform.eulerAngles);

        //相对父对象角度
        print(this.transform.localEulerAngles);

        //注意：设置角度和设置位置一样 不能单独设置xyz 要一起设置
        //如果我们希望改变的 角度 是面板上显示的内容 那一点是改变 相对父对象的角度
        this.transform.localEulerAngles = new Vector3(10, 10, 10);
        this.transform.eulerAngles = new Vector3(10, 10, 10);
        print(this.transform.localEulerAngles);
        #endregion
    }

    void Update()
    {
        #region 知识点二 旋转相关
        //自己计算（省略不讲了 和位置一样 不停改变角度即可）

        //API计算
        //自转
        //每个轴 具体转多少度
        //第一个参数 相当于 是旋转的角度 每一帧 
        //第二个参数 默认不填 就是相对于自己坐标系 进行的旋转
        this.transform.Rotate(new Vector3(0, 10, 0) * Time.deltaTime);
        this.transform.Rotate(new Vector3(0, 10, 0) * Time.deltaTime, Space.World);

        //相对于某个轴 转多少度
        //参数一：是相对哪个轴进行转动
        //参数二：是转动的 角度 是多少
        //参数三：默认不填 就是相对于自己的坐标系 进行旋转
        //       如果填  可以填写相对于 世界坐标系进行旋转
        this.transform.Rotate(Vector3.right, 10 * Time.deltaTime);
        this.transform.Rotate(Vector3.right, 10 * Time.deltaTime, Space.World);

        //相对于某一个点转
        //参数一：相当于哪一个点 转圈圈
        //参数二：相对于那一个点的 哪一个轴转圈圈
        //参数三：转的度数  旋转速度 * 时间
        this.transform.RotateAround(Vector3.zero, Vector3.right, 10 * Time.deltaTime);
        #endregion
    }

    //总结
    //角度相关和位置相关 差不多
    //如何得到角度
    //通过transform 可以得到相对于世界坐标系和相对于父对象的
    //如何自转和绕着别人赚
    //Rotate
    //RotateAround
}

```

#### Transform——缩放和看向

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson8 : MonoBehaviour
{
    public Transform lookAtObj;

    void Start()
    {
        #region 知识点一 缩放
        //相对世界坐标系
        print(this.transform.lossyScale);
        //相对本地坐标系（父对象）
        print(this.transform.localScale);

        //注意：
        //1.同样缩放不能只改xyz 只能一起改(相对于世界坐标系的缩放大小只能得 不能改)
        //所以 我们一般要修改缩放大小 都是改的 相对于父对象的 缩放大小 localScale
        this.transform.localScale = new Vector3(3, 3, 3);

        //2.Unity没有提供关于缩放的API
        //之前的 旋转 位移 都提供了 对应的 API 但是缩放并没有
        #endregion
    }
    
    void Update()
    {
        //2.Unity没有提供关于缩放的API
        //之前的 旋转 位移 都提供了 对应的 API 但是 缩放并没有
        //如果你想要 让 缩放 发生变化 只能自己去写(自己算)
        this.transform.localScale += Vector3.one * Time.deltaTime;

        #region 知识点二 看向
        //让一个对象的面朝向 可以一直看向某一个点或者某一个对象
        //看向一个点 相对于世界坐标系的
        this.transform.LookAt(Vector3.zero);
        //看向一个对象 就传入一个对象的  Transform信息
        this.transform.LookAt(lookAtObj);
        #endregion
    }

    //总结：
    //缩放相关
    //相对于 世界坐标系的缩放 只能得 不能改
    //只能去修改相对于本地坐标系的缩放（相对于父对象）
    //没有提供对应的API来 缩放变化 只能自己算
    //看向
    //LookAt 看向一个点 或者一个对象  
    //一定记住 是写在Update里面 才会不停变化
}

```

#### Transform——父子关系

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson9 : MonoBehaviour
{
    public Transform son;

    void Start()
    {
        #region 知识点一 获取和设置父对象
        //获取父对象
        print(this.transform.parent.name);
        //设置父对象 断绝父子关系
        this.transform.parent = null;
        //设置父对象 新建父子关系
        this.transform.parent = GameObject.Find("Father2").transform;

        //通过API来进行父子关系的设置
        this.transform.SetParent(null);//断绝父子关系
        this.transform.SetParent(GameObject.Find("Father2").transform);//新建父子关系

        //参数一：父对象
        //参数二：是否保留世界坐标的 位置 角度 缩放 信息
        //       true  会保留 世界坐标下的状态  和 父对象 进行计算 得到本地坐标系的信息
        //       false 不会保留 会直接把世界坐标系下的 位置角度缩放 直接赋值到 本地坐标系下 
        this.transform.SetParent(GameObject.Find("Father3").transform, false);
        #endregion

        #region 知识点二 抛妻弃子
        //就是和自己的所有儿子 断绝关系 没有父子关系了
        this.transform.DetachChildren();
        #endregion

        #region 知识点三 获取子对象
        //按名字查找儿子
        //找到儿子的 transform信息
        //Find方法 是能够找到 失活的对象的 GameObject相关的 查找 是不能找到失活对象的
        print(this.transform.Find("Cube (1)").name);

		/*
		 * 可以通过路径查找子对象的孩子
		 */
		print(this.transform.Find("Rank/Rank1").name);
        
        //他只能找到自己的儿子 找不到自己的孙子
        //虽然它的效率 比GameObject.Find相关 要高一些 但是 前提是你必须知道父亲是谁 才能找

        //遍历儿子
        //如何得到有多少个儿子
        //1.失活的儿子也会算数量
        //2.找不到孙子 所以孙子不会算数量
        print(this.transform.childCount);
        
        //通过索引号 去得到自己对应的儿子
        //如果编号 超出了儿子数量的范围 那会直接报错的 
        //返回值 是 transform 可以得到对应儿子的 位置相关信息
        this.transform.GetChild(0);

        for (int i = 0; i < this.transform.childCount; i++)
        {
            print("儿子的名字：" + this.transform.GetChild(i).name);
        }
        #endregion

        #region 知识点四 儿子的操作
        //判断自己的父对象是谁
        //一个对象 判断自己是不是另一个对象的儿子
        if(son.IsChildOf(this.transform))
        {
            print("是我的儿子");
        }
        
        //得到自己作为儿子的编号
        print(son.GetSiblingIndex());
        
        //把自己设置为第一个儿子
        son.SetAsFirstSibling();
        
        //把自己设置为最后一个儿子
        son.SetAsLastSibling();
        
        //把自己设置为指定个儿子
        //就算你填的数量 超出了范围（负数或者更大的数） 不会报错 会直接设置成最后一个编号
        son.SetSiblingIndex(1);
        #endregion
    }
    //总结
    // 设置父对象相关的内容
    // 获取子对象

    //抛弃妻子
    //儿子的操作
}

```

#### Transform——坐标转换

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson10 : MonoBehaviour
{
    void Start()
    {
        #region 知识点一 世界坐标转本地坐标
        print(Vector3.forward);

        //世界坐标系 转本地坐标系 可以帮助我们大概判断一个相对位置

        //世界坐标系的点 转换 为相对本地坐标系的点  
        //受到缩放影响
        print("转换后的点 " + this.transform.InverseTransformPoint(Vector3.forward));

        //世界坐标系的方向 转换 为相对本地坐标系的方向 
        //不受缩放影响
        print("转换后的方向" + this.transform.InverseTransformDirection(Vector3.forward));
        
        //受缩放影响
        print("转换后的方向(受缩放影响)" + this.transform.InverseTransformVector(Vector3.forward));
        #endregion

        #region 知识点二 本地坐标转世界坐标
        //本地坐标系的点 转换 为相对世界坐标系的点 受到缩放影响
        print("本地 转 世界 点" + this.transform.TransformPoint(Vector3.forward));

        //本地坐标系的方向 转换 为相对世界坐标系的方向 
        //不受缩放影响
        print("本地 转 世界 方向" + this.transform.TransformDirection(Vector3.forward));
        
        //受缩放影响
        print("本地 转 世界 方向" + this.transform.TransformVector(Vector3.forward));
        #endregion
    }

    //总结
    // 其中最重要的 就是 本地坐标系的点 转世界坐标系的点
    // 比如 现在玩家要在自己面前的n个单位前 放一团火 这个时候 我不用关心世界坐标系
    // 通过 相对于本地坐标系的位置 转换为 世界坐标系的点 进行 特效的创建 或者 攻击范围的判断
}

```