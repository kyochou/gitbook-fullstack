# CLI

## Pipeline
* `> /dev/null 2&>1`: 抛弃所有输出.

### xargs
一些 cli 程序不能接受标准输入作为参数, 只能直接在命令行输入参数, 这导致无法用管道命令传递参数. `xargs` 命令的作用, 就是将标准输入转为命令行参数.

#### Refs
* [xargs 命令教程](http://www.ruanyifeng.com/blog/2019/08/xargs-tutorial.html)


## Bash
### FAQs
* 查看进程打开的文件(包括端口): `lsof -Pp <pid>`.
* 查看端口被哪个程序占用: `lsof -sTCP:LISTEN -Pi:<port>` 或 `netstat -aenp | grep <port>`.
* 查看命令是否可存在: `command -v <cmd>`.
* 使用脚本为命令设置环境变量:
    当我们在 Shell 中运行一个命令时, 该 Shell 就会 Fork 出一个新进程来执行命令. 也就是说, 新进程是子 Shell, 而之前的 Shell 是父 Shell. 在子 Shell 中设置的变量是无法传递到父 Shell 或其同级的 Shell 中的.
    如果想在命令执行前设置相应的环境变量, 比如代理设置, 需要在命令前指定环境变量, 如:
    ```shell
    HTTP_PROXY=http://127.0.0.1:19999 <CMD>
    ```
    如果不想每次执行命令都进行这样的设置, 可以将命令与环境变量的设置包装进一个脚本中执行, 内容如下:
    
    ```bash
    #/bin/bash
    # kproxy
    HTTP_PROXY=http://127.0.0.1:19999 
    $@  
    ```
    这样, 以后就可以使用 `kproxy <CMD>` 这种方式为命令添加环境变量了.