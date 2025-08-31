### 热更新概念
游戏或者软件更新时
无需重新下载客户端进行安装
而是在应用程序启动的情况下
在内部进行的资源或者代码更新

### 热更新优点
迅速修复Bug——避免重新下载安装包，游戏内部更新修复Bug
提升玩家留存率——避免玩家因为超大的安装包而流失

### AB包理论基础
#### 优点
- 相对Resous下的资源，AB包更好管理
- ![[Resources和AB包对比.png]]
- 减少包体大小
	- 压缩资源
	- 减少初始包大小
- 热更新
	- 资源热更新
	- 脚本热更新
	- ![[热更新基本规则.png]]

### AB包资源打包
![[生成AB包资源文件.png]]
AB包生成的文件
- AB包文件
	- 资源文件
- manifest文件
	- AB包文件信息
	- 当加载时，提供资源信息、依赖关系、版本信息等关键信息
- 关键AB包（和目录名一样的包）
	- 主包
	- AB包依赖关键信息

### AB包资源加载
第一步：加载AB包
```c#
AssetBundle ab = AssetBundle.LoadFromFile(Application.StreamingAssetPath + "/model");

//卸载所有Ab包
AssetBundle.UnloadAllAssetBundle(false);
//卸载单个Ab包
ab.Unload(false);
```

第二步：加载AB包中的资源
```c#
//同步加载
GameObject obj = ab.LoadAsset("Cube", typeof(GameObject)) as GameObject;

//异步加载
StartCoroutine(LoadABRes("model", "Cube"))
IEnumerator LoadABRes(string ABName, string resName)
{
	AssetBundleCreateRequest abcr = AssetBundle.LoadFromFileAsync(Application.StreamingAssetPath + "/" + ABName);
	yield return abcr;
	AssetBundleRequest abr = abcr.assetBundle.LoadAssetAsync(resName, typeof(GameObject));
	yield return abr;
	GameObject obj = abr.asset as Gameobject;
}
```

AB包不能重复加载，否则会报错

### AB包依赖
需要将依赖和被依赖的ab包都被加载
利用主包获取依赖信息
1. 加载主包
2. 加载主包中的固定文件
3. 从固定文件中获得依赖信息