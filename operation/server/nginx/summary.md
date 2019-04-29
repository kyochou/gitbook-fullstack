# Nginx

Nginx 的组成:
* 二进制可执行文件.
* nginx.conf 配置文件.
* access.log 访问日志.
* error.log 错误日志. 

版本: 与 Linux 内核版本一样, 偶数结尾的版本表示稳定版.

## 架构
Nginx 为了减少系统上下文切换, 它的 worker 是用单进程单线程设计的. 事实证明这种做法运行效率很高.

### 变量
在 Nginx 配置中,变量只能存放一种类型的值, 就是字符串.

```nginx
set $h 'hello';
set $w 'world';
# 在变量定义中引用已存在的值, 称为 "变量插值(variable interpolation)".
set $hw "$h ${w}";
echo $hw;
```

并非所有的配置指令都支持 "变量插值". 取决于该指令的实现模块.
**使用 `set` 指令创建变量并为其赋值. 需要注意的是, Nginx 变量的创建和赋值操作发生在不同的时间阶段. 创建只能发生在配置加载或者说 Nginx 启动的时候; 而赋值操作则只会发生在请求实际处理的时候**. 
**Nginx 变量一旦创建, 其变量名的可见范围就是整个 Nginx 配置. 但每个请求都会单独维护各自的所有变量的独立副本, 彼此互不干扰. 即 Nginx 变量的生命周期是不可能跨越请求边界的**.
一个请求在其处理过程中, 即使经历多个不同的 location 配置块, 它使用的还是同一套 Nginx 变量副本. `rewrite` 指令和 `echo_exec` 指令可以发起这种 "内部跳转".


### 模块
core module 用于定义一些共用的代码.
[模块分类](https://files-kyo.oss-cn-hongkong.aliyuncs.com/Fsyv8R6OmAMyaVG1daTw7uwhdB4V.png)
[模块结构](https://files-kyo.oss-cn-hongkong.aliyuncs.com/FjN0S0KGglfq8_zIgTHUgj5QEhOD.png)

#### 动态模块
动态模块可以使我们在升级 Nginx 时减少编译环节.
Linux 中文件后缀为 `.a` 的为静态模块, 后缀为 `.so` 的为动态模块.
编译时(`configure`)添加如 `--with-http_realip_module=dynamic` 形式的参数可以将模块编译为动态模块.
在配置文件中使用 `load_module` 指令开启动态模块.


### Pool
#### 连接池
#### 内存池
通过使用内存池, 可减少内存碎片的数量.

### 事件
Nginx 的事件驱动框架使用了 ET(边沿) 模式的 Epoll, 以实现更高的性能. 
Nginx 往往不能容忍大量消耗 CPU 的第三方模块. 因大量的 CPU 运算会导致 Nginx 处理事件的时间过长, 从而致使事件队列发生堵塞.
Linux 中所有的系统调用都需要做用户态与内核态间的切换. Nginx Work 进程在同一进程中同时处理多个连接, 在用户态代码中完成了连接的切换, 从而减少了 OS 进程切换成本.

### 进程
虽然 Nginx 有单进程和多进程两个启动方式, 但在生产环境中, 一般都是使用多进程模式.
多进程(相比多线程)具有更健壮的高可靠性和高可用性.
Nginx 的进程中, Master Process 为主进程, Work 进程和 Cache 进程都是它的子进程. 各进程之间通过共享内存进行通信.
Nginx 期望每个 Work 进程可以占用一个固定的逻辑处理器(绑定在一起), 以提高处理效率.

#### 通讯
Nginx 进程间通讯的方式有: 信号, 共享内存, 锁, Slab 内存管理器.
共享内存是 Nginx 跨 Work 进程通信的最有效手段.
ngx_slab_stat 模块可以统计 Slab 内存管理器的内存使用情况.


### 信号
Work 进程也可以接收信号, 但我们一般只是给 Master 进程发送信号即可.
Linux 系统规定, 当子进程终止时, 会向父进程发送 CHLD 信号.
TERM, INI 信号表示立刻停止进程, QUIT 信号表示让进程自已退出, 允许其在退出前执行一些操作.

![Nginx 进程管理](https://files-kyo.oss-cn-hongkong.aliyuncs.com/Fvtp_YA5-DNryqv8MedBU8wCgai8.png)

### 源码
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
        
* 查看编译到 Nginx 中的模块:

    在 `configure` 命令执行后, 查看 `objs/ngx_modules.c` 文件. 里面有模块相关的设置.
    
* 通过源码查看模块支持的指令:

    在源码文件中(如 `src/http/modules/ngx_http_gzip_filter_module.c`), 查找类型为 `ngx_command_t` 的结构体, 在其内有模块相关指令的定义.    
        
### 热部署
即在不停止服务的基础上实现 Nginx 的版本升级. 步骤如下:
1. 下载新的源码并编译(不要安装).
2. 备份正在运行的 sbin/nginx 文件, 然后使用新编译的文件覆盖它.
3. 给正在运行的 Nginx 的 master 进程发送热部署信号: `kill -USR2 <master-pid>`. 这样, Nginx 就会使用新的 sbin/nginx 文件启动一个新的 master 进程. 注意, 原 master 进程并不会自动退出, 此时会有二个 master 进程在运行. 但老的 master 进程已经不再监听端口, 即不再接受新的请求了.

    老的 Master 进程会为其 PID 文件后缀改为 `oldbin`, 然后使用新的  sbin/nginx 文件启动新 Master 进程.

1. 给旧的 master 进程发送关闭 work 进程的信号: `kill -WINCH <old-master-pid>`.
2. 保留二个 master 进程同时运行一段时间. 如果新版本没有问题, 可执行 `kill -QUIT <old-master-pid>` 将旧的 master 进程杀掉; 如果新版本有问题, 则向旧的 Master 发送 HUP 信号(reload), 向新的 Maseter 发送 QUIT 信号.

![热升级流程](https://files-kyo.oss-cn-hongkong.aliyuncs.com/FjARsDjOsMf2RuXwBw9EsXz0y_SQ.png)

    
### CLI
Nginx 只有短命令参数(`-`), 没有长命令参数(`--`).
`nginx -t` 可以用来测试配置文件是否有语法错误.
Nginx 操作运行中进程的方法一般是给进程发送信号. 可通过 `kill` 命令或 `nginx -s` 命令来实现. 
`nginx -s` 可接收的参数有:
* stop: 立即停止服务(`kill -9`).
* quit: 发送退出信号, 由 Nginx 自行退出(`kill`).
* reload: 重载配置文件.
* reopen: 重新开始记录所有日志文件.

### 重载配置文件流程(reload)
1. 向 Master 进程发送 HUP 信号(reload 命令)
2. Master 进程校验配置语法是否正确
3. Master 进程打开新的监听端口, 关闭不再使用的端口.

    在 Linux 中, 子进程会继承父进程的所有的已打开的端口.
    即使不再监听端口, 已经建立的连接仍会正确返回.
    
1. Master 进程用新配置启动新的 Work 子进程.
2. Master 进程向老 Work 子进程发送 QUIT 信号.
3. 老 Work 进程关闭监听句柄, 处理完当前连接后结束进程.

![reload 流程](https://files-kyo.oss-cn-hongkong.aliyuncs.com/FslMhLGFwWOdycdQ5APuRKykDVIa.png)

### Nginx 优雅停止的流程
1. 在配置中设定 `worker_shutdown_timeout` 的值用于控制 Work 进程的最大关闭时间(可选).
2. 进程关闭监听句柄.
3. 关闭空闲连接.
4. 在循环中等待全部连接的关闭.
5. 退出进程(Work 全部关闭或 `worker_shutdown_timeout` 到期).
    
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

#### 指令
* `root` 和 `alias` 指令都可以指定目录和访问路径的映射关系. 他们的主要区别在于是否会使用配置路径中的匹配部分.
* `autoindex` 可打开目录和文件的浏览功能.
* `limit_*` 相关指令可实现限速功能.
* gzip

    ```nginx
    # Gzip Settings
    gzip on;
    # 如果存在对应的 .gz 后缀的文件则直接返回
    gzip_static on;
    # 小于这个值的文件不压缩
    gzip_min_length 1k;
    gzip_buffers 4 16k;
    # 压缩比, 1-9
    gzip_comp_level 2;
    # 文本类型文件的的压缩比会比较高
    gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/javascript application/x-httpd-php;
    ```
    
* 给 `listen` 指令加 IP 前缀可以限制请求的来源.
* `upstream` 指令结合 `proxy_pass` 指令可以将请求转发给上游服务(反向代理).
    * `proxy_set_header` 指令用于设置向上游服务发送的请求头.
    * `proxy_cache` 系列指令可以将上游服务的返回信息缓存在本地.


### log 配置
日志文件名的配置支持使用变量, 以时间变量进行配置即可实现日志的自动切隔.
手动更新日志文件:
1. 将日志文件重命名(`mv access.log old.log`), 注意要使用 `mv` 命令而不是 `cp`. **在 Linux 文件系统中, 改名并不会影响已经打开的文件的写入操作, 因其文件句柄 inode 没有改变**, 这样就不会出现丢日志现象了.
2. 给 nginx 发送 reopen 信号(`nginx -s reopen` 或 `kill -USR1 <nginx-pid>`).

日志格式通过 `log_format` 指令设置, 可为其指定名称以便于引用.


## Tools
[goaccess](https://github.com/allinurl/goaccess/): 实时的 Web 日志分析工具. 可在命令行中显示.

## Refs
* [nginx documentation](http://nginx.org/en/docs/)
* [Nginx 核心知识 100 讲](https://time.geekbang.org/course/detail/138-65084)
* [geektime-geekbang/geektime-nginx](https://github.com/geektime-geekbang/geektime-nginx)
* [openresty/nginx-tutorials](https://github.com/openresty/nginx-tutorials)