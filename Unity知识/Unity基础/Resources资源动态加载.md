### 特殊文件夹

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson16 : MonoBehaviour
{
    void Start()
    {
        #region 知识点一 工程路径获取
        //注意：该方式获取到的路径，一般情况下只在编辑模式下使用
        //游戏发布过后该路径就不存在了
        
        print(Application.dataPath);
        
        #endregion

        #region 知识点二 Resources 资源文件夹
        //路径获取：
        //一般不获取
        //只能使用Resources相关API进行加载
        //如果硬要获取 可以用工程路径拼接print(Application.dataPath + "/Resources");

        //注意：需要我们自己将创建
        //作用：资源文件夹
        //1-1.需要通过Resources相关API动态加载的资源需要放在其中
        //1-2.该文件夹下所有文件都会被打包出去
        //1-3.打包时Unity会对其压缩加密
        //1-4.该文件夹打包后只读 只能通过Resources相关API加载
        #endregion

        #region 知识点三 StreamingAssets 流动资源文件夹
        //路径获取：
        
        print(Application.streamingAssetsPath);
        
        //注意：需要我们自己将创建
        //作用：流文件夹
        //2-1.打包出去不会被压缩加密
        //2-2.移动平台只读，PC平台可读可写
        //2-3.可以放入一些需要自定义动态加载的初始资源
        #endregion

        #region 知识点四 persistentDataPath 持久数据文件夹
        //路径获取：
        
        print(Application.persistentDataPath);

        //注意：不需要我们自己将创建
        //作用：固定数据文件夹
        //3-1.所有平台都可读可写
        //3-2.一般用于放置动态下载或者动态创建的文件，游戏中创建或者获取的文件都放在其中
        #endregion

        #region 知识点五 Plugins 插件文件夹
        //路径获取：一般不获取

        //注意：需要我们自己将创建
        //作用：插件文件夹
        //不同平台的插件相关文件放在其中
        //比如IOS和Android平台
        #endregion

        #region 知识点六 Editor 编辑器文件夹
        //路径获取：一般不获取
        //如果硬要获取 可以用工程路径拼接
        print(Application.dataPath + "/Editor");

        //注意：需要我们自己将创建
        //作用：编辑器文件夹
        //5-1.开发Unity编辑器时，编辑器相关脚本放在该文件夹中
        //5-2.该文件夹中内容不会被打包出去
        #endregion

        #region 知识点七 默认资源文件夹 Standard Assets
        //路径获取：一般不获取

        //注意：需要我们自己将创建
        //作用：默认资源文件夹
        //一般Unity自带资源都放在这个文件夹下
        //代码和资源优先被编译
        #endregion
    }
}

```

### Resources同步加载

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson17 : MonoBehaviour
{
    public AudioSource audioS;

    private Texture tex;

    // Start is called before the first frame update
    void Start()
    {
        #region 知识点一 Resources资源动态加载的作用
        //1.通过代码动态加载Resources文件夹下指定路径资源
        //2.避免繁琐的拖曳操作
        #endregion

        #region 知识点二 常用资源类型
        //1.预设体对象——GameObject
        //2.音效文件——AudioClip
        //3.文本文件——TextAsset
        //4.图片文件——Texture
        //5.其它类型——需要什么用什么类型

        //注意：
        //预设体对象加载需要实例化
        //其它资源加载一般直接用
        #endregion

        #region 知识点三 资源同步加载 普通方法
        //在一个工程当中 Resources文件夹可以有多个，通过API加载时，它会自己去这些同名的Resources文件夹中去找资源
        //打包时 Resources文件夹里的内容都会打包在一起

        //1.预设体对象 想要创建在场景上
        // 第一步：要去加载预设体的资源文件(本质上 就是加载 配置数据 在内存中)
        Object obj = Resources.Load("Cube");
        //第二步：如果想要在场景上 创建预设体 一定是加载配置文件过后 然后实例化
        Instantiate(obj);

        //2.音效资源
        //第一步：就是加载数据
        Object obj3 = Resources.Load("Music/BKMusic");
        //第二步：使用数据 我们不需要实例化 音效切片 我们只需要把数据 赋值到正确的脚本上即可
        audioS.clip = obj3 as AudioClip;
        audioS.Play();

        //3.文本资源
        //文本资源支持的格式
        //.txt  .xml  .bytes  .json  .html  .csv ...
        TextAsset ta = Resources.Load("Txt/Test") as TextAsset;
        //文本内容
        print(ta.text);
        //字节数据组
        print(ta.bytes);

        //4.图片
        tex = Resources.Load("Tex/TestJPG") as Texture;

        //5.其它类型 需要什么类型 就用什么类型就行

        //6-1加载指定类型的资源
        tex = Resources.Load("Tex/TestJPG", typeof(Texture)) as Texture;
        ta = Resources.Load("Tex/TestJPG", typeof(TextAsset)) as TextAsset;


        //6-2加载指定名字的所有资源
        Object[] objs = Resources.LoadAll("Tex/TestJPG");
        foreach (Object item in objs)
        {
            if (item is Texture){ 
            }
            else if(item is TextAsset){
            }
        }

        // 资源同步加载 泛型方法
        TextAsset ta2 = Resources.Load<TextAsset>("Tex/TestJPG");

        #region 总结
        //Resources动态加载资源的方法
        //让拓展性更强 相对拖曳来说 它更加一劳永逸 更加方便

        //重要知识点：
        //记住API
        //记住一些特定的格式
        //预设体加载出来一定要实例化
        #endregion
    }

    private void OnGUI()
    {
        GUI.DrawTexture(new Rect(0, 0, 100, 100), tex);
    }
}

```

### Resources异步加载

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson18 : MonoBehaviour
{
    private Texture tex;
    
    void Start()
    {
        #region 知识点一 Resources异步加载是什么？
        //上节课学习的同步加载中
        //如果我们加载过大的资源可能会造成程序卡顿
        //卡顿的原因就是 从硬盘上把数据读取到内存中 是需要进行计算的
        //越大的资源耗时越长，就会造成掉帧卡顿

        //Resources异步加载 就是内部新开一个线程进行资源加载 不会造成主线程卡顿
        #endregion

        #region 知识点二 Resources异步加载方法
        //注意：
        //异步加载 不能马上得到加载的资源 至少要等一帧

        //1.通过异步加载中的完成事件监听 使用加载的资源
        //这句代码 你可以理解 Unity 在内部 就会去开一个线程进行资源下载
        ResourceRequest rq = Resources.LoadAsync<Texture>("Tex/TestJPG");
        //马上进行一个 资源下载结束 的一个事件函数监听
        rq.completed += LoadOver;

        //这个 刚刚执行了异步加载的 执行代码 资源还没有加载完毕，一定要等加载结束过后 才能使用

        //2.通过协程 使用加载的资源
        StartCoroutine(Load());
        #endregion

        #region 总结
        //1.完成事件监听异步加载
        //好处：写法简单
        //坏处：只能在资源加载结束后 进行处理
        //“线性加载”

        //2.协程异步加载
        //好处：可以在协程中处理复杂逻辑，比如同时加载多个资源，比如进度条更新
        //坏处：写法稍麻烦
        //“并行加载”

        //注意：
        //理解为什么异步加载不能马上加载结束，为什么至少要等1帧
        //理解协程异步加载的原理
        #endregion
    }

    IEnumerator Load()
    {
        //迭代器函数 当遇到yield return时  就会 停止执行之后的代码
        //然后 协程协调器 通过得到 返回的值 去判断 下一次执行后面的步骤 将会是何时
        ResourceRequest rq = Resources.LoadAsync<Texture>("Tex/TestJPG");

        //第一部分
        //Unity 自己知道 该返回值 意味着你在异步加载资源 
        // yield return rq;
        //Unity 会自己判断 该资源是否加载完毕了 加载完毕过后 才会继续执行后面的代码
        print(Time.frameCount);
        
        //判断资源是否加载结束
        while(!rq.isDone)
        {
            //打印当前的 加载进度 
            //该进度 不会特别准确 过渡也不是特别明显
            print(rq.progress);
            yield return null;
        }
        tex = rq.asset as Texture;
    }

    private void LoadOver( AsyncOperation rq )
    {
        print("加载结束");
        //asset 是资源对象 加载完毕过后就能够得到它
        tex = (rq as ResourceRequest).asset as Texture;
    }

    private void OnGUI()
    {
        if( tex != null)
            GUI.DrawTexture(new Rect(0, 0, 100, 100), tex);
    }
}

```

### Resources资源卸载

```c#
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Lesson19 : MonoBehaviour
{
    private Texture tex;
    
    void Start()
    {
        #region 知识点一 Resources重复加载资源会浪费内存吗？
        //其实Resources加载一次资源过后
        //该资源就一直存放在内存中作为缓存
        //第二次加载时发现缓存中存在该资源
        //会直接取出来进行使用
        //所以 多次重复加载不会浪费内存
        //但是 会浪费性能（每次加载都会去查找取出，始终伴随一些性能消耗）

        #endregion

        #region 知识点二 如何手动释放掉缓存中的资源
        //1.卸载指定资源
        //Resources.UnloadAsset 方法
        //注意：
        //该方法 不能释放 GameObject对象 因为它会用于实例化对象
        //它只能用于一些 不需要实例化的内容 比如 图片 和 音效 文本等等
        //一般情况下 我们很少单独使用它
        //即使是没有实例化的 GameObject对象也不能进行卸载
        //GameObject obj = Resources.Load<GameObject>("Cube"); ×
        //Resources.UnloadAsset(obj); ×
        Resources.UnloadAsset(tex);

        //2.卸载未使用的资源
        //注意：一般在过场景时和GC一起使用
        Resources.UnloadUnusedAssets();
        GC.Collect();
        
        #endregion

        #region 总结
        //Resources.UnloadAsset 卸载指定资源 但是不能卸载GameObject对象
        //Resources.UnloadUnusedAssets 卸载未使用资源 一般过场景时配合GC使用
        #endregion
    }

    void Update()
    {
        if(Input.GetKeyDown(KeyCode.Alpha1))
        {
            print("加载资源");
            tex = Resources.Load<Texture>("Tex/TestJPG");
        }
        if(Input.GetKeyDown(KeyCode.Alpha2))
        {
            print("卸载资源");
            Resources.UnloadAsset(tex);
            tex = null;
        }
    }
}
```