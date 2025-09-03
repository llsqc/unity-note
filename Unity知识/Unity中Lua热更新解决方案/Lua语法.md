### 第一个Lua程序
```lua
-- 单行注释
--[[
多行注释
]] --
print("hello world")
```

### 变量
```lua
-- lua当中的简单变量类型
-- nil number string boolean
-- 通过 type 函数可以得到变量的类型

-- nil 类似null
a = nil
-- number 所有的数值
a = 1
a = 1.2
-- string 字符串
a = "hello world"
a = 'hello lua'
-- boolean 布尔值
a = true
a = false

-- lua中使用没有声明过的变量不会报错，默认值是nil
print(b)
print(type(b))

-- 复杂数据类型
-- 函数 function
-- 表 table
-- 数据结构 userdata
-- 协同程序 thread(线程)
```

### 字符串操作
```lua
str = "双引号字符串"
str2 = '单引号字符串'

-- 一个汉字占3个长度
s = "aBcdEfG字符串"
-- 获取字符串的长度
print(#s)

-- lua中支持转义字符
print("hello\nworld")

-- lua的多行字符串
s = [[hello
lua
]]
print(s)

-- 字符串拼接
-- 字符串拼接 通过..
s1 = 111
s2 = 111
print(s1 .. s2)

print(string.format("我今年%d岁了", 18))
-- %d :与数字拼接
-- %a：与任何字符拼接
-- %s：与字符配对
-- .......

-- 别的类型转字符串
a = true
print(tostring(a))

-- 字符串提供的公共方法
str = "abCdefgCd"

-- 小写转大写的方法
print(string.upper(str))

-- 大写转小写
print(string.lower(str))

-- 翻转字符串
print(string.reverse(str))

-- 字符串索引查找（索引从1开始）
print(string.find(str, "Cde"))

-- 截取字符串
print(string.sub(str, 3, 4))

-- 字符串重复
print(string.rep(str, 2))

-- 字符串修改
print(string.gsub(str, "Cd", "**"))

-- 字符转 ASCII码
print(string.byte("Lua", 1))

-- ASCII码 转字符
print(string.char("L"))
```

### 运算符
```lua
-- 运算符

-- 算数运算符
-- + - * / % ^
-- 没有自增自减 ++ --
-- 没有复合运算符 += -= /= *= %=
-- 字符串可以进行算数运算符操作，会自动转成number
-- ^ lua中，该符号是幂运算
print("幂运算" .. 2 ^ 5)
print("123.4" ^ 2)

-- 条件运算符
-- > < >= <= == ~=
-- 不等于 是 ~=
print(3 ~= 1)

-- 逻辑运算符
-- and  or  not  lua中遵循逻辑运算的 “短路” 规则
print(false and true)
print(true or false)
print(not true)

-- 位运算符
-- & | 不支持位运算符 需要我们自己实现

-- 三目运算符
-- ? :  lua中 也不支持 三目运算
```

### 条件分支语句
```lua
-- 条件分支语句
a = 9
-- if 条件 then.....end
-- 单分支
if a > 5 then
    print("a > 5")
end

-- 双分支
-- if 条件 then.....else.....end
if a < 5 then
    print("a < 5")
else
    print("a >= 5")
end

-- 多分支
-- if 条件 then.....elseif 条件 then....elseif 条件 then....else.....end
if a < 5 then
    print("a < 5")
elseif a == 6 then
    print("a == 6")
elseif a == 7 then
    print("a == 7")
else
    print("other")
end

if a >= 3 and a <= 9 then
    print("3到9之间")
end

-- lua中没有switch语法，需要自己实现
```

### 循环语句
```lua
-- 循环语句

-- while语句
num = 0
-- while 条件 do ..... end
while num < 5 do
    print(num)
    num = num + 1
end

-- do while语句
num = 0
-- repeat ..... until 条件 （注意：条件是结束条件）
repeat
    print(num)
    num = num + 1
until num > 5 -- 满足条件跳出 结束条件

-- for语句
for i = 2, 5 do -- 默认递增 i会默认+1
    print(i)
end

for i = 1, 5, 2 do -- 如果要自定义增量 直接逗号后面写
    print(i)
end

for i = 5, 1, -1 do -- 如果要自定义增量 直接逗号后面写
    print(i)
end
```

### 函数
```lua
-- 函数
-- function 函数名()
-- end

-- a = function()
-- end

-- 无参数无返回值
function F1()
    print("F1函数")
end
F1()

F2 = function()
    print("F2函数")
end
F2()

-- 有参数
function F3(a)
    print(a)
end
F3("lua")

-- 如果你传入的参数和函数参数个数不匹配
-- 不会报错，只会补空nil或者丢弃

-- 有返回值
function F4(a)
    return a, "lua", true
end
-- 多返回值时，在前面声明多个变量来接取即可
-- 如果变量不够，值接取对应位置的返回值
-- 如果变量多了，直接赋nil
temp, temp2, temp3, temp4 = F4("1")
print(temp)
print(temp2)
print(temp3)
print(temp4)

-- 函数的类型
-- 函数类型 就是 function
print(type(F4))

-- 函数的重载
-- lua中函数不支持重载 
-- 默认调用最后一个声明的函数

-- 变长参数
function F5(...)
    -- 变长参数使用：用一个表存起来
    arg = {...}
    for i = 1, #arg do
        print(arg[i])
    end
end
F5(1, "lua", true, 4, 5, 6)

-- 函数嵌套
function F6()
    return function()
        print(123);
    end
end
f6 = F6()
f6()

-- 闭包
function F7(x)
    -- 改变传入参数的生命周期
    return function(y)
        return x + y
    end
end

f7 = F7(10)
print(f7(5))
```

### 表
#### 数组
```lua
-- 复杂数据类型 table
-- 所有的复杂类型都是table（表）

-- 数组
a = {1, 2, nil, 3, "1231", true, nil}

-- lua中，索引从1开始
-- 在使用#获取长度的时候 nil被视为数组的结束
print(#a)

-- 数组的遍历
for i = 1, #a do
    -- 使用#获取长度不可靠
    print(a[i])
end

-- 二维数组
b = {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}}

-- 自定义索引
c = {
    [1] = "1",
    [2] = "2",
    [3] = "3",
}

-- 迭代器遍历
a = {
    [0] = 1,
    2,
    [-1] = 3,
    4,
    5,
    [5] = 6
}

-- ipairs迭代器遍历
-- ipairs遍历是从1开始往后遍历的，小于等于0的值得不到
-- 只能找到连续索引的键，如果中间断序了无法遍历出后面的内容
for i, k in ipairs(a) do
    print("ipairs遍历键值" .. i .. "_" .. k)
end
-- ipairs迭代器遍历键
for i in ipairs(a) do
    print("ipairs遍历键" .. i)
end

-- pairs迭代器遍历
-- 它能够把所有的键都找到 通过键可以得到值
for i, v in pairs(a) do
    print("pairs遍历键值" .. i .. "_" .. v)
end
-- pairs迭代器遍历键
for i in pairs(a) do
    print("pairs遍历键" .. i)
end
```

#### 字典
```lua
-- 字典
-- 字典的声明
-- 字典是由键值对构成 
a = {
    ["name"] = "tony",
    ["age"] = 14,
    ["1"] = 5
}

-- 访问当个变量 用中括号填键 来访问
print(a["name"])

-- 还可以类似.成员变量的形式得到值
print(a.age)

-- 虽然可以通过.成员变量的形式得到值 但是不能是数字
print(a["1"])

-- 修改
a["name"] = "mike";

-- 新增
a["sex"] = false

-- 删除
a["sex"] = nil

-- 字典的遍历pairs
for k, v in pairs(a) do
    print(k, v)
end

for k in pairs(a) do
    print(k)
    print(a[k])
end

for _, v in pairs(a) do
    print(_, v)
end
```

#### 类和结构体
```lua
-- 类和结构体
-- Lua中是默认没有面向对象的，需要我们自己来实现成员变量、成员函数等

Student = {
    age = 1,
    sex = true,
    -- 成长函数
    Grow = function()
        -- 一定要指定是谁的 使用 表名.属性 或 表名.方法
        print(Student.age)
    end,
    -- 学习函数
    Learn = function(t)
        -- 或者把自己作为一个参数传进来 在内部访问
        print(t.sex)
    end
}

-- Lua中 .和冒号的区别
Student.Learn(Student)
-- 冒号调用方法，会默认把调用者作为第一个参数传入方法中
Student:Learn()

-- 声明表过后 在表外去声明表有的变量和方法
Student.name = "roxy"
Student.Speak = function()
    print("说话")
end

-- 函数的第三种声明方式
function Student:Speak2()
    -- lua中 关键字 self 表示默认传入的第一个参数
    print(self.name .. "说话")
end
```

#### 表的公共方法
```lua
-- 表的公共操作
-- 表中 table提供的一些公共方法的讲解

t1 = {{
    age = 1,
    name = "123"
}, {
    age = 2,
    name = "345"
}}

t2 = {
    name = "roxy",
    sex = true
}

-- 插入
table.insert(t1, t2);

-- 移除
-- 删除指定元素
-- remove方法 会移除最后一个索引的内容
table.remove(t1)

-- remove方法 传两个参数 第一个参数 是要移除内容的表
-- 第二个参数 是要移除内容的索引
table.remove(t1, 1)

-- 排序
t2 = {5, 2, 7, 9, 5}

-- 传入要排序的表 默认升序排列
table.sort(t2)
for _, v in pairs(t2) do
    print(v)
end

-- 降序
-- 传入两个参数 第一个是用于排序的表 第二个是排序规则函数
table.sort(t2, function(a, b)
    if a > b then
        return true
    end
end)

for _, v in pairs(t2) do
    print(v)
end

-- 拼接
tb = {"123", "456", "789"}
-- 连接函数 用于拼接表中元素 返回值 是一个字符串
str = table.concat(tb, ",")
print(str)
```

### 多脚本执行
```lua
-- 多脚本执行
-- 全局变量和本地变量
-- 本地（局部）变量的关键字 local
for i = 1, 2 do
    local str = "roxy"
    print(str)
end
print(str)

-- 多脚本执行
-- 关键字 require("脚本名")
require('Test')
print(testA)
-- 其他脚本的local变量无法被访问
print(testLocalA)

-- 脚本卸载
-- 如果是require加载执行的脚本 加载一次过后不会再被执行
-- package.loaded["脚本名"] 返回值是boolean 意思是 该脚本是否被执行
print(package.loaded["Test"])

-- 卸载已经执行过的脚本
package.loaded["Test"] = nil

-- require 执行一个脚本时 可以在脚本最后 return 一个外部希望获取的内容

-- 大G表
-- _G表是一个总表(table) 他将我们声明的所有全局的变量都存储在其中
for k, v in pairs(_G) do
    print(k, v)
end
-- 本地变量 加了local的变量时不会存到大_G表中
```

### 特殊用法
```lua
-- 特殊用法
-- 多变量赋值 如果后面的值不够会自动补空，多了会自动省略
local a, b, c = 1, 2, "123"
-- 多返回值函数同理

-- and or
-- 逻辑与 逻辑或
-- and or 他们不仅可以连接 boolean 任何东西都可以用来连接
-- 在lua中 只有 nil 和 false 才认为是假
print(true or 1)

-- lua不支持三目运算符，通过 and or 实现
x = 1
y = 2
-- ? :
local res = (x > y) and x or y
print(res)
```

### 协同程序
```lua
-- 协同程序
-- 协程的创建

-- 常用方式
-- coroutine.create()
fun = function()
    print(123)
end
co = coroutine.create(fun)
-- 协程的本质是一个线程对象

-- coroutine.wrap()
co2 = coroutine.wrap(fun)

-- 协程的运行

-- 第一种方式 co为thread
coroutine.resume(co)

-- 第二种方式 co2为function
co2()

-- 协程的挂起
fun2 = function()
    local i = 1
    while true do
        print(i)
        i = i + 1
        -- 协程的挂起函数
        coroutine.yield(i)
    end
end

-- 默认第一个返回值是协程是否启动成功
-- yield里面的返回值
co3 = coroutine.create(fun2)
isOk, tempI = coroutine.resume(co3)
print(isOk, tempI)

-- 这种方式的协程调用 也可以有返回值 只是没有默认第一个返回值了
co4 = coroutine.wrap(fun2)
print("返回值" .. co4())

-- 协程的状态

-- coroutine.status(协程对象)
-- dead 结束
-- suspended 暂停
-- running 进行中
print(coroutine.status(co3))

-- 这个函数可以得到当前正在 运行的协程的线程号
print(coroutine.running())
```

### 元表
```lua
-- 元表
-- 元表概念
-- 任何表变量都可以作为另一个表变量的元表，有自己的元表
-- 当我们子表中进行一些特定操作时，会执行元表中的内容

-- 设置元表
meta = {}
myTable = {}
-- 设置元表函数 第一个参数 子表，第二个参数 元表
setmetatable(myTable, meta)

-- 特定操作
-- 特定操作-__tostring
meta2 = {
    -- 当子表要被当做字符串使用时 会默认调用这个元表中的tostring方法
    __tostring = function(t)
        return t.name
    end
}
myTable2 = {
    name = "roxy"
}
setmetatable(myTable2, meta2)
print(myTable2)

-- 特定操作-__call
meta3 = {
    __tostring = function(t)
        return t.name
    end,
    -- 当子表被当做一个函数来使用时 会默认调用这个__call中的内容
    -- 默认第一个参数 是调用者自己
    __call = function(a, b)
        print(a)
        print(b)
    end
}
myTable3 = {
    name = "roxy"
}
setmetatable(myTable3, meta3)
-- 把子表当做函数使用 就会调用元表的 __call方法
myTable3(1)

-- 特定操作-运算符重载
meta4 = {
    __add = function(t1, t2)
        return t1.age + t2.age
    end,
    __sub = function(t1, t2)
        return t1.age - t2.age
    end,
    __mul = function(t1, t2)
        return 1
    end,
    __div = function(t1, t2)
        return 2
    end,
    __mod = function(t1, t2)
        return 3
    end,
    __pow = function(t1, t2)
        return 4
    end,
    __eq = function(t1, t2)
        return true
    end,
    __lt = function(t1, t2)
        return true
    end,
    __le = function(t1, t2)
        return false
    end,
    -- 运算符..
    __concat = function(t1, t2)
        return "hello world"
    end
}
myTable4 = {
    age = 1
}
myTable5 = {
	age = 2
}
setmetatable(myTable4, meta4)
setmetatable(myTable5, meta4)
print(myTable4 + myTable5)
print(myTable4 .. myTable5)
-- 如果要用条件运算符来比较两个对象，两个对象的元表一定要一致
print(myTable4 == myTable5)

-- 特定操作-__index和__newIndex

myTable6 = {}
meta6 = {
}
meta6Father = {
    age = 1
}
-- __index的赋值，写在表外面来初始化
meta6.__index = meta6
meta6Father.__index = meta6Father
setmetatable(myTable6, meta6)
setmetatable(meta6, meta6Father)
-- __index 当子表中找不到某一个属性时 
-- 会到元表中 __index指定的表去找属性，一层层往上找
print(myTable6.age)

-- newIndex 当赋值时，如果赋值一个不存在的索引
-- 那么会把这个值赋值到newindex所指的表中 不会修改自己
meta7 = {}
meta7.__newindex = {}
myTable7 = {}

setmetatable(myTable7, meta7)
myTable7.age = 1
print(myTable7.age)

-- 得到元表的方法
print(getmetatable(myTable6))

-- rawget 当我们使用它是 会去找自己身上有没有这个变量
print(rawget(myTable6, "age"))

-- rawset 该方法 会忽略newindex的设置 只会该自己的变量
rawset(myTable7, "age", 2)
```

### Lua面向对象
#### 封装
```lua
-- 封装
-- 面向对象 类 其实都是基于 table来实现
-- 元表相关的知识点
Object = {}
function Object:Test()
    print(self.id)
end

function Object:new()
    -- self 代表的是 我们默认传入的第一个参数
    -- 对象就是变量 返回一个新的变量
    -- 返回出去的内容 本质上就是表对象
    local obj = {}
    -- 元表知识 __index 当找自己的变量 找不到时 就会去找元表当中__index指向的内容
    self.__index = self
    setmetatable(obj, self)
    return obj
end

local myObj = Object:new()
```

#### 继承
```lua
-- 继承
-- 用于继承的方法
function Object:subClass(className)
    -- _G知识点 是总表 所有声明的全局标量 都以键值对的形式存在其中
    _G[className] = {}

    -- 写相关继承的规则
    local obj = _G[className]
    self.__index = self
    -- 子类 定义个base属性 base属性代表父类
    obj.base = self
    setmetatable(obj, self)
end

Object:subClass("Person")
local p1 = Person:new()
```

#### 多态
```lua
-- 多态
-- 相同行为 不同表象 就是多态
-- 相同方法 不同执行逻辑 就是多态
Object:subClass("GameObject")
GameObject.posX = 0;
GameObject.posY = 0;
function GameObject:Move()
    self.posX = self.posX + 1
    self.posY = self.posY + 1
    print(self.posX)
    print(self.posY)
end

GameObject:subClass("Player")
function Player:Move()
    -- base 指的是 GameObject 表（类）
    -- 这种方式调用 相当于是把基类表 作为第一个参数传入了方法中
    -- 避免把基类表 传入到方法中 这样相当于就是公用一张表的属性了
    -- 我们如果要执行父类逻辑 我们不要直接使用冒号调用
    -- 要通过.调用 然后自己传入第一个参数 
    -- self.base:Move()
    self.base.Move(self)
end

local p1 = Player:new()
p1:Move()
```

#### 实现Object
```lua
-- 面向对象实现 
-- 万物之父 所有对象的基类 Object

-- 封装
Object = {}

-- 实例化方法
function Object:new()
    local obj = {}

    -- 给空对象设置元表 以及 __index
    self.__index = self
    setmetatable(obj, self)
    return obj
end

-- 继承
function Object:subClass(className)

    -- 根据名字生成一张表 就是一个类
    _G[className] = {}
    local obj = _G[className]

    -- 设置自己的“父类”
    obj.base = self

    -- 给子类设置元表 以及 __index
    self.__index = self
    setmetatable(obj, self)
end

--[[
实践
]] --
-- 申明一个新的类

Object:subClass("GameObject")

-- 成员变量
GameObject.posX = 0
GameObject.posY = 0

-- 成员方法
function GameObject:Move()
    self.posX = self.posX + 1
    self.posY = self.posY + 1
end

-- 实例化对象使用
local obj = GameObject:new()
print(obj.posX)
obj:Move()
print(obj.posX)

-- 申明一个新的类 Player 继承 GameObject
GameObject:subClass("Player")

-- 多态 重写了 GameObject的Move方法
function Player:Move()
    -- base调用父类方法 用.自己传第一个参数
    self.base.Move(self)
end
print("****")

-- 实例化Player对象
local p1 = Player:new()
print(p1.posX)
p1:Move()
print(p1.posX)
```

### 自带库
```lua
-- 自带库
-- 时间
-- 系统时间
print(os.time())

-- 自己传入参数 得到时间
print(os.time({
    year = 2014,
    month = 8,
    day = 14
}))

-- os.date("*t")
local nowTime = os.date("*t")
for k, v in pairs(nowTime) do
    print(k, v)
end
print(nowTime.hour)

-- 数学运算
-- math
-- 绝对值
print(math.abs(-11))

-- 弧度转角度
print(math.deg(math.pi))

-- 三角函数 传弧度
print(math.cos(math.pi))

-- 向下向上取整
print(math.floor(2.6))
print(math.ceil(5.2))

-- 最大最小值
print(math.max(1, 2))
print(math.min(4, 5))

-- 小数分离 分成整数部分和小数部分
print(math.modf(1.2))
 
-- 幂运算
print(math.pow(2, 5))

-- 随机数
-- 先设置随机数种子
math.randomseed(os.time())
print(math.random(100))

-- 开方
print(math.sqrt(4))

-- 路径
-- lua脚本加载路径
package.path = package.path .. ";C:\\"
```

### 垃圾回收
```lua
-- 垃圾回收
test = {
    id = 1,
    name = "123123"
}

-- 垃圾回收关键字
-- collectgarbage
-- 获取当前lua占用内存数 K字节 用返回值*1024 就可以得到具体的内存占用字节数
print(collectgarbage("count"))

-- lua中的机制和C#中垃圾回收机制很类似 置空就是变垃圾
test = nil

-- 进行垃圾回收
collectgarbage("collect")

-- lua中 有自动定时进行GC的方法
-- Unity中热更新开发 尽量不要去用自动垃圾回收
```