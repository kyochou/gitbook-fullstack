# Make
make 命令依赖 Makefile 文件进行构建. 它的规则很简单, 你规定要构建哪个文件, 它依赖哪些源文件, 当那些文件有变动时, 如何重新构建它.

## Makefile
注释行用 `#` 开头.
使用 `%` 可对文件名进行模式匹配.
允许使用 `=` 自定义变量.
调用 Shell 变量时, 变量需要放在 `$()` 或 `${}` 之中.
Makefile 文件由一系列规则构成. 每条规则的形式如下:

```makefile
.PHONY: <target>
<target>: <prerequisites>
[tab] <commands>
```

### Target
目标(target)是必需的, 不可省略; 前置条件(prerequisites)和命令(commands)都是可选的, 但是至少要有一个.
目标通常是文件名, 指明 make 命令所要构建的对象. 目标可以是多个文件名, 之间用空格分隔.
如果当前目录中, 存在和目标同名的文件, 则 make 命令不会执行. 此时可以为目标指定一个别名(伪目标), 通过 `.PHONY` 定义. 声明目标为伪目标后, make 就不会去检查是否存在同名的文件.
每条规则就明确两件事: 构建目标的前置条件是什么, 以及如何构建.
如果 make 命令没有指定目标, 则默认执行第一个目标.

### Prerequisites
前置条件通常是一组文件名, 之间用空格分隔. 它指定了 "目标" 是否重新构建的判断标准: 只要有一个前置文件不存在, 或者有过更新(前置文件的 last-modification 时间戳比目标的时间戳新), "目标" 就需要重新构建.

### Commands
命令由一行或多行 Shell 组成. 它是构建 "目标" 的具体指令.
每行命令前必须要有一个 tab 键. 如果想用其他键, 可使用 `.RECIPEPREFIX` 声明.
需要注意的是, 每行命令都在一个单独的 Shell 中执行, 这些 Shell 之间没有继承关系. 使用 `.ONESHELL` 可以改变这种行为.
正常情况下, make 会打印每条命令, 然后再执行. 这叫做回声(echoing), 通过在命令前加 `@` 可以关闭回声.



## Refs
* [Make 命令教程](http://www.ruanyifeng.com/blog/2015/02/make.html)
* [跟我一起写 Makefile](https://seisman.github.io/how-to-write-makefile/index.html)