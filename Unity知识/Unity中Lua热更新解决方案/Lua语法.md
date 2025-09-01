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
-- 多返回值时，在前面申明多个变量来接取即可
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
-- 字典的申明
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