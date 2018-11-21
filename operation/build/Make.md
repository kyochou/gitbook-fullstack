# Make
make 命令依赖 Makefile 文件进行构建. 它的规则很简单, 你规定要构建哪个文件, 它依赖哪些源文件, 当那些文件有变动时, 如何重新构建它.

## Makefile
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

## Refs
* [Make 命令教程](http://www.ruanyifeng.com/blog/2015/02/make.html)