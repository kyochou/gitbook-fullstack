# Bash
## 变量
### 定义
* 指定默认值: `FOO=${VARIABLE:-default}  # If variable not set or null, use default.`
* 指定默认值, 并赋予引用变量(同时为两个变量赋值): `FOO=${VARIABLE:=default}  # If variable not set or null, set it to default.`

## 参数

* $0: 脚本文件名.
* $<n>: n 为数字, 表示传递给文件的第 n 个参数.
* $#: 传递参数的个数.
* $*: 传递给脚本的所有参数的字符串值.
* $@: 传递给脚本的所有参数的数组值.

### FAQs
* 参数默认值: port=${1:-9000}

### 命令
* shift \<n\>: 将参数数组(`$@`)左移 n 位. 默认值为 1. 

## 特殊值
* $_: 上一个命令的最后一个参数.
* $?: 上个命令的退出状态或函数的返回值.
* $$: 当前 shell 的进程 ID.
* $!: 上一个进入后台运行的进程的 ID.

## Script
### FAQs
* 使用带空格路径的变量

```bash
code='/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code'
# 注意使用引号
"$code"  --install-extension yzhang.markdown-all-in-one
```