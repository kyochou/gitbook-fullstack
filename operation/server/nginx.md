# Nginx

Nginx 的组成:
* 二进制可执行文件.
* nginx.conf 配置文件.
* access.log 访问日志.
* error.log 错误日志. 

版本: 与 Linux 内核版本一样, 偶数结尾的版本表示稳定版.


## 源码
* 将源码中 `contrib/vim/` 目录下的文件复制到 `~/.vim/` 目录下, 可以使文件 nginx.conf 中的关键字高亮显示(只能高亮显示这一个配置文件).

    ```shell
    cp -r contrib/vim/* ~/.vim/
    ```
    
* 开启 debug 级别的 error_log:

    1. 编译时(configure)添加 `--with-debug` 选项.
    2. 在 nginx.conf 文件中设置 error_log 的级别为 debug:

        ```nginx
        error_log  logs/error.log  debug;
        ```
    
## 配置
语法: 
* 配置文件由指令与指令块构成.
* 每条指令以 `;` 分号结尾, 指令与参数间以空格符号分隔.
* 指令块以 `{}` 大括号将多条指令组织在一起.
* `include` 语句允许组合多个配置文件以提升可维护性.
* 使用 `#` 符号添加注释.
* 使用 `$` 符号使用变量.
* 部分指令的参数支持正则表达式.

时间单位:
* ms: milliseconds.
* s: seconds.
* m: minutes.
* h: hours.
* d: days.
* w: weeks.
* M: months.
* y: years.

空间(大小)单位:
* 默认: 什么都不写, 表示 bytes.
* k/K: kilobytes, kb.
* m/M: megabytes, mb.
* g/G: gigabytes. gb.

### http 配置
http 配置包括以下指令块:
* server
* location
* upstream

### log 配置
日志文件名的配置支持使用变量, 以时间变量进行配置即可实现日志的自动分隔.


## OpenResty



## Refs
* [Nginx 核心知识 100 讲](https://time.geekbang.org/course/detail/138-65084)
* [geektime-geekbang/geektime-nginx](https://github.com/geektime-geekbang/geektime-nginx)