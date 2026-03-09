### 热补丁步骤

```lua
--我们必须做4个非常重要的操作
--1.加特性
-- [XLua.Hotfix]
--2.加宏 第一次开发热补丁需要加
-- HOTFIX_ENABLE
--3.生成代码
--4.hotfix 注入  --注入时可能报错 提示你要引入Tools

--热补丁的缺点：只要我们修改了热补丁类的代码，我们就需要重新执行第4步需要重新点击 注入

--lua当中 热补丁代码固定写法
--xlua.hotfix(类, "函数名", lua函数)

--成员函数 第一个参数 self
xlua.hotfix(CS.HotfixMain, "Add", function(self, a, b)
	return a + b
end)

--静态函数 不用传第一个参数
xlua.hotfix(CS.HotfixMain, "Speak", function(a)
	print(a)
end)
```

### 多函数替换

```lua
--多函数替换

--lua当中 热补丁代码固定写法
--xlua.hotfix(类, "函数名", lua函数)

--xlua.hotfix(类, {函数名 = 函数, 函数名 = 函数....})
xlua.hotfix(CS.HotfixMain, {
	Update = function(self)
		print(os.time())
	end,
	Add = function(self, a, b )
		return a + b
	end,
	Speak = function(a)
		print(a)
	end
})

xlua.hotfix(CS.HotfixTest, {
	--构造函数 热补丁固定写法[".ctor"]
	--他们和别的函数不同 不是替换 是先调用原逻辑 再调用lua逻辑
	[".ctor"] = function()
		print("Lua热补丁构造函数")
	end,
	Speak = function(self,a)
		print("Hello" .. a)
	end,
	--析构函数固定写法Finalize
	Finalize = function()
		
	end
})
```

### 协程函数

```lua
-- 协程函数替换

--xlua.hotfix(类, {函数名 = 函数, 函数名 = 函数....})
--要在lua中配合C#协程函数  那么必使用它
util = require("xlua.util")
xlua.hotfix(CS.HotfixMain, {
	TestCoroutine = function(self)
		--返回一个正儿八经的 xlua处理过的lua协程函数
		return util.cs_generator(function()
			while true do
				coroutine.yield(CS.UnityEngine.WaitForSeconds(1))
				print("Lua打补丁后的协程函数")
			end
		end)
	end
})
```

### 索引器和属性替换

```lua
--属性和索引器替换

xlua.hotfix(CS.HotfixMain, {
	--如果是属性进行热补丁重定向
	--set_属性名 是设置属性的方法
	--get_属性名 是得到属性的方法
	set_Age = function(self, v)
		print("Lua重定向的属性"..v)
	end,
	get_Age = function(self)
		return 10;
	end,
	--索引器固定写法
	--set_Item 通说索引器设置
	--get_Item 通过索引器获取
	set_Item = function(self, index, v )
		print("Lua重定向索引器，索引："..index .."值" ..v)
	end,
	get_Item = function(self, index)
		print("Lua重定向索引器")
		return 999
	end
})
```

### 事件操作替换

```lua
-- 事件加减替换

xlua.hotfix(CS.HotfixMain, {
	--add_事件名 代表着事件加操作
	--remove_事件名 减操作
	add_myEvent = function(self, del)
		print(del)
		print("添加事件函数")
		--会去尝试使用lua使用C#事件的方法去添加
		--在事件加减的重定向lua函数中
		--千万不要把传入的委托往事件里存
		--否则会死循环
		--会把传入的函数存在lua中
		--self:myEvent("+", del)
	end,
	remove_myEvent = function(self, del )
		print(del)
		print("移除事件函数")
	end
})
```

### 泛型类替换

```lua
--泛型类的替换
--lua中的替换 要一个类型一个类型的来

xlua.hotfix(CS.HotfixTest2(CS.System.String), {
	Test = function(self, str)
		print("lua中打的补丁:"..str)
	end
})

xlua.hotfix(CS.HotfixTest2(CS.System.Int32), {
	Test = function(self, str)
		print("lua中打的补丁:"..str)
	end
})
```