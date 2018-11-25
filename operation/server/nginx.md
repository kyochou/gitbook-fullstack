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
        
### 热部署
即在不停止服务的基础上实现 Nginx 的版本升级. 步骤如下:
1. 下载新的源码并编译(不要安装).
2. 备份正在运行的 sbin/nginx 文件, 然后使用新编译的文件覆盖它.
3. 给正在运行的 Nginx 的 master 进程发送热部署信号: `kill -USR2 <master-pid>`. 这样, Nginx 就会使用新的 sbin/nginx 文件启动一个新的 master 进程. 注意, 原 master 进程并不会自动退出, 此时会有二个 master 进程在运行. 但老的 master 进程已经不再监听端口, 即不再接受新的请求了.
4. 给旧的 master 进程发送关闭 work 进程的信号: `kill -WINCH <old-master-pid>`.
5. 保留二个 master 进程同时运行一段时间. 如果新版本没有问题, 可执行 `kill -QUIT <old-master-pid>` 将旧的 master 进程杀掉; 如果新版本有问题, 则重新执行一遍上述流程, 将新的 sbin/nginx 替换成旧的.
    
## CLI
Nginx 只有短命令参数(`-`), 没有长命令参数(`--`).
`nginx -t` 可以用来测试配置文件是否有语法错误.
Nginx 操作运行中进程的方法一般是给进程发送信号. 可通过 `kill` 命令或 `nginx -s` 命令来实现. 
`nginx -s` 可接收的参数有:
* stop: 立即停止服务(`kill -9`).
* quit: 发送退出信号, 由 Nginx 自行退出(`kill`).
* reload: 重载配置文件.
* reopen: 重新开始记录所有日志文件.

```shell
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


#### Refs
* [Module ngx_http_core_module](http://nginx.org/en/docs/http/ngx_http_core_module.html)

### log 配置
日志文件名的配置支持使用变量, 以时间变量进行配置即可实现日志的自动切隔.
手动更新日志文件:
1. 将日志文件重命名(`mv access.log old.log`), 注意要使用 `mv` 命令而不是 `cp`. **在 Linux 文件系统中, 改名并不会影响已经打开的文件的写入操作, 因其文件句柄 inode 没有改变**, 这样就不会出现丢日志现象了.
2. 给 nginx 发送 reopen 信号(`nginx -s reopen` 或 `kill -USR1 <nginx-pid>`).

日志格式通过 `log_format` 指令设置, 可为其指定名称以便于引用.


## OpenResty



## Refs
* [Nginx 核心知识 100 讲](https://time.geekbang.org/course/detail/138-65084)
* [geektime-geekbang/geektime-nginx](https://github.com/geektime-geekbang/geektime-nginx)