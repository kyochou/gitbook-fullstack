# Lua
相同目的的操作优先使用 openresty 中提供的库. 通过会有更好的性能.
Lua 语言的设计目的是为了嵌入到应用程序中, 从而为应用程序提供灵活的扩展和定制功能.
LuaJIT 利用即时编译技术把 Lua 代码编译成本地机器码后交由 CPU 直接执行, 以获得更快的运行速度.
Lua 提供了一个虚变量(dummy variable)`_`, 用来做为变量的占位符.

## 基础数据类型
`type` 函数能够返回一个值或变量所属的类型.

* nil: 表示 "无效值". 一个变量的初始值为 `nil`, 将 `nil` 赋予一个变量就等同于删除它.
* boolean: Lua 中的 `nil` 和 `false` 为假, 其它值均为真.
* number: 表示实数. 可以使用数学函数 `math.floor`, `math.ceil` 进行取整.
* string: Lua 中有三种表示字符串的形式, 双引号, 单引号和长括号`[[]]`(相当于 `HEREDOC`). Lua 中的字符串是不可改变的值, 也不能通过下标来访问字符串的某个字符. 
* table: 关联数组. 索引可以是除了 `nil` 以外的任意类型的值. 使用 `{}` 定义.
* function: 具名函数的定义本质上是匿名函数对变量的赋值.

## 表达式
* `^`: 指数(次方)运算.
* `~=`: 不等于.
* `==`: 等于. 在进行相等判断时, 对于 table, userdate 和函数, Lua 是作引用比较的.
* 字符串连接: `..` 操作符或使用 `string.format`. 如果有很多连接操作, 推荐使用 `table.concat` 函数.
* `#`: 取字符串或表的长度. `#str, #table`.


### 逻辑运算

* `and`: 逻辑与.
* `or`: 逻辑或.
* `not`: 逻辑非.

所有逻辑操作符会将 `false` 和 `nil` 视作假, 其他任何值视作真. 对于 `and` 和 `or`, 短路求值, 对于 `not`, 永远只返回 `true` 或者 `false`.

短路求值: 
* `a and b`: 如果 a 为假, 则返回 a, 否则返回 b.
* `a or b`: 如果 a 为假, 则返回 b, 否则返回 a.

## 控制结构

```lua
if ... then
    ...
elseif ... then -- 注意 else 和 if 是写在一起的
    ...
else
    ...
end

```

Lua 没有提供 `continue` 关键字, 只可使用 `break` 跳出循环.

```lua
while ... do
    ...
end    

-- repeat 循环, 直到 until 条件为真是才结束
repeat
    ...
until ...

-- 数字型 for
-- begin, finish, step 表达式只会在循环开始时执行一次.
-- step 默认为 1.
-- 循环过程中不要改变控制变量 var 的值.
for var = begin, finish, step do
    ...
end
for i = 1, 5 do
    print(i)
end    

-- 泛型 for
-- 通过循环迭代器(iterator)函数来遍历值.
-- 迭代器: pairs, ipairs, io.lines, string.gmatch ...
for key in pairs(table) do
    ...
end
for key, value in ipairs(table) do
    ...
end    
    
```

## 函数
由于函数定义本质上就是变量赋值, 而变量的定义总是应放置在变量使用之前, 所以函数的定义也需要放置在函数调用之前.

由于函数定义等价于变量赋值, 可以直接把函数名替换为变量或 table 的 key: 
```lua
local tbl = {}

function tbl.func()
    ...
end
-- 等价于
tbl.func = function()
    ...
end    
    
```

### 参数

Lua 函数的参数大部分是按值传递的. `table` 类型参数为引用传递.
在调用函数时, 若形参个数与实参个数不同时, Lua 会自动调整. 多余的自动忽略, 少的会被初始化为 `nil`.

使用 `...` 可定义变长参数:

```lua
local function func(...)
    print(...)
    local args = {...}
    print(args[1])
end
    
```

为 Lua 函数只指定一个参数时, 此参数可做为 `table` 类型解析, 从而实现具名参数的效果:

```lua
local function func(tbl)
    print(tbl.width, tbl.length)
end
func({width = 10, length = 5})
```

### 返回值
函数可返回多个值, 用 `,` 分隔.
如果函数调用不是一个列表表达式的最后一个元素, 那么函数调用只会产生一个返回值(第一个):
```lua
local function init()
    return 1, 'lua'
end

local x, y, z = init(), 2 -- init 函数不是最后一个调用, 此时只返回 1.
print(x, y, z) -- 1, 2, nil
local x, y, z = 2, init() -- init 函数的位置在最后, 此时返回 1 和 'lua'
print (x, y, z) -- 2, 1, 'lua'

-- 函数调用的实参列表也是一个列表表达式
print(init(), 2) -- 1, 2
print(2, init()) -- 2, 1, 'lua'

-- 如果你只需要取函数的第一个返回值, 可使用括号运算符
print((init()), 2) -- 1, 2
print(2, (init())) -- 2, 1
```
如果实参列表中某个函数会返回多个值, 同时调用者又没有显式地使用括号运算符来筛选和过滤, 则这样的表达式是不能被 LuaJIT 所 JIT 编译的, 只能被解释执行.

一般情况下, `return` 只能作为函数的最后一行语句. 但如果 `return` 行后只有一行语句, 这行语句会被提升到和 `return` 同一行执行并返回.
可使用 `do return end` 写法在其后添加多条语句, 但不会被执行.

## 模块
通过 `require` 函数引用模块文件. 模块文件中通过 `return` 语句声明需要导出的变量.

## String 库
可 Lua5.1 中, String 库可以直接作为 string 类型的方法使用:
```lua
local str = 'string'
-- 下面两个使用方法等价
print(str:upper(), string.upper(str))
```

Lua 字符串总是由字节构成. Lua 核心并不尝试理解具体的字符集编码.

* string.byte(s[, start[, end]]): 返回字符所对应的 ASCII 码. start 默认为 1, j 默认为 start. 使用此函数进行字符串相关的扫描和分析是最为高效的.
* string.char(...): 接收不定参数个整数(0-255), 默认为 0. 返回这些整数对应的 ASCII 码字符组成的字符串. 
* string.uppser(s), string.lower(s).
* string.len(s): 此函数不推荐使用. 应当总是使用 `#` 运算符来获取 Lua 字符串长度.
* string.find(s, p[, init[, plain]])
* string.format(formatstring, ...)
* string.match(s, p[, init])
* string.gmatch(s, p): 全局匹配. 此函数目前只能解释执行. 应尽量使用 `ngx.re.gmatch` 等操作.
* string.rep(s, n): 返回字符串 s 的 n 次拷贝.
* string.sub(s, i[, j]): 返回子串.
* string.gsub(s, p, r[ ,n]): 字符串替换.
* string.reverse(s): 返回反转字符串.


## Table
与其他语言不同的是, Lua 中表的索引是自 1 开始.

```lua
local tbl = {
    -- 索引为字符串
    web = 'www.google.com',
    ['city'] = 'hefei',
    -- 相当于 [1] = 123
    123,
    -- 指定索引的值
    [10] = 456,
}

```

Lua 内部实际采用哈希表和数组分别保存键值对, 普通值, 所以不推荐混合使用数字和字符串做为健值.
不要在表中使用 `nil` 值. 如果一个元素要删除, 直接 `remove`, 不要用 `nil` 去替代.

* table.getn: 获取长度. 使用 `#` 操作符取表的长度会有一定的不确定性.
* table.concat: 将表中值合并为字符串.
* table.insert
* table.maxn: 返回表中最大索引编号.
* table.remove
* table.sort
* table.new: 预分配表空间.
* table.clear: 高效释放表空间.


### Metatable
在 Lua5.1 中, 元表的表现行为类似于 C++ 语言中的操作符重载.


## 其他函数库
### 日期时间
推荐使用 `ngx_lua` 模块提供的带缓存的时间接口. 如 `ngx.today`, `ngx.time` 等.

### 数学
math 库.