# Cofiguration

执行 `reload` 操作, 并不会清空共享内存.

## Variables
变量的提供(定义)模块定义变量名和解析出变量的方法(在 `preconfiguration` 中定义新变量).
变量的使用模块(config 文件)根据变量的值进行相应处理. 变量的值是其指定方法的返回值, 是处于变化状态的.

### Nginx 系统变量
* time_local: 本地时间格式的当前时间.
* time_iso8601: ISO 8601 格式的当前时间.
* nginx_version: Nginx 版本号.
* pid: worker 进程的 PID.
* pipe: 使用了管道则返回 `p`, 否则返回空.
* hostname: 所在服务器的主机名.
* msec: unixtime.

### TCP 相关变量
* binary_remote_addr: 客户端地址的整型格式.
* connection: 递增的连接序号.
* connection_requests: 当前连接上执行过的请求数, 对 keepalive 连接有意义.
* remote_addr: 客户端地址.
* remote_port: 客户端端口.
* proxy_protocol_addr, proxy_protocol_port: 若使用了 proxy_protocol 协议则返回协议中的值.
* server_addr: 服务端地址.
* server_port: 服务端端口.
* TCP_INFO: TCP 内核层参数.
* server_protocol: 服务器协议. 如 HTTP/1.1.

### HTTP 请求相关的变量
* host: 主机名. 其取值如下:
    * 自请求行中获取.
    * 自请求头的 `Host` 值中获取, 如果存在则覆盖请求行中的值.
    * 如果前两者都取不到, 使用匹配上的 `server_name`.
* http_头部名字: 返回指定的请求头的值. 特殊处理的头部有 `http_host`, `http_user_agent`, `http_referer`, `http_via`, `http_x_forwarded_for`, `http_cookie`. 
* args, query_string: 全部 URL 参数.
* arg_参数名: URL 中某个具体参数的值. Nginx 会在匹配参数名之前, 自动把原始请求中的参数名调整为全部小写的形式.
* is_args: 如果请求 URL 中有参数则返回 `?`, 否则返回空.
* content_length: 请求头中 Content-Length 的值.
* content_type: 请求头中 Content-Type 的值.
* uri, document_uri: 请求的 URI(不包括 `?` 后的参数).
* request: 原始的 URL 请求行. 包含方法与协议版本.
* request_uri: 请求的 URL(包括 URI 及完整参数).
* request_length: 所有请求内容的大小, 包括请求行, 请求头, 请求体等.
* remote_user: 由 HTTP Basic Authentication 协议传入的用户名.
* request_body_file: 临时存放请求体的文件.
* scheme, request_method, request_body...

### Nginx 处理请求过程中产生的变量
* request_time: 请求处理到现在的耗时.
* server_name: 匹配上请求的 server_name.
* https: 如果开启了 TLS/SSL, 则返回 `on`.
* request_completion: 若请求处理完则返回 `OK`, 否则返回空.
* request_id: 随机生成的请求 ID.
* request_filename: 待访问文件的完整路径.
* document_root: 由 URI 和 `root/alias` 规则生成的文件夹路径.
* realpath_root: 将 `document_root` 中的软链接等转换成真实路径后的值.
* limit_rate: 返回客户端响应时的速度上限.


### HTTP 响应相关的变量
* body_bytes_sent: 响应中 body 体的长度.
* bytes_sent: 全部 http 响应的长度.
* status: http 响应中的返回码.
* sent_trailer_名称: 返回响应结尾内容中的值.
* sent_http_头部全称: 返回响应中 http header 的值. 特殊处理的有 `sent_http_content_type`, `sent_http_content_length`, `sent_http_location`, `sent_http_last_modified`, `sent_http_connection`, `sent_http_keep_alive`, `sent_http_transfer_encoding`, `sent_http_cache_control`, `sent_http_link`.

### 指令
* variables_hash_bucket_size
* variables_hash_max_size


## Directives
* 值指令: 用于存储配置项的值. 可以被覆盖.
* 动作类指令: 用于指定行为. 不能被覆盖.

### 执行顺序 
Nginx 处理每个用户请求时, 都会按照 11 个处理阶段(phase)依次执行. 所有 Nginx 模块提供的配置指令一般只会注册并运行在其中的某一个处理阶段. 所以, **指令的执行顺序与其在 Nginx 配置中出现的顺序无关, 而与其所属的处理阶段有关; 同一阶段不同模块指令的执行顺序与其所属模块在 `ngx_modules.c` 中注册的顺序有关; 同一模块同一处理阶段的指令则会依次序执行**.

### 指令集
* listen: 监听一个地址或端口.

### if
一般来说避免使用 if 指令是个好主意.
if 指令中尽量避免非 `rewrite` 指令的出现.
如果一个配置块中出现了多个同级的 if 指令, 注意前面的 if 指令是否会被覆盖.

### location
使用 `internal` 指令可将其限制为只允许内部访问.

### 定义
* Context: 指定指令可以出现的模块.
