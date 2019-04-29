# HTTP

## HTTP 请求处理的 11 个阶段

Nginx 通过变量在模块间传递数据.
同一阶段不同模块的执行顺序是与 `ngx_modules.c` 文件中模块的定义(`ngx_module_names` 变量)顺序相反的. 与其在配置文件中出现的次序无关.

### POST_READ
Nginx 读取完所有的请求头部之后, 没有做任何再加工前.

* realip 模块

    `X-Real-IP` 用于传递用户 IP.
    `X-Forwarded-For` 用于传递代理 IP.

### REWRITE
* return 指令. 可以帮助我们做重定向或直接返回.

    ```nginx
    return code [text];
    return code URL;
    return URL;
    ```
    return 码为 `444` 会直接使 Nginx 关闭连接, 客户端是收不到响应的.
    
* rewrite 指令. 

    ```nginx
    rewrite regex replacement [flag];
    ```    
    将匹配 `regex` 的 url 替换成 replacement. 可以使用正则表达式及变量提取.
    当 `replacement` 以 `http://`, `https://` 或者 `$schema` 开头, 则直接返回 302 重定向.
    替换后的 URL 根据 `flag` 指定的方式处理:
    * 默认不会做任何事, 继续执行当前块代码.
    * last: 中断当前块的执行并用 `replacement` 这个 URI 进行新的 location 匹配.
    * break: 停止当前脚本指令的执行, 直接返回 `replacement` 对应的资源 (不会再进行 location 匹配).
    * redirect: 返回 302 重定向.
    * permanent: 返回 301 重定向.
* rewrite_log 指令.
    将其设为 `on` 会在 error_log 中记录 rewrite 的相关操作.  
* if 指令.

    ```nginx
    if (condition) {...}
    ```
    condition:
        * 直接使用变量, 为空或 0 时值为 false.
        * 使用 `=` 或 `!=` 判断.
        * 正则匹配: `~` 或 `!~`. 
        * 大小写不敏感的正则匹配: `~*` 或 `!~*`.
        * 检查文件是否存在: `-f` 或 `!-f`.
        * 检查目录是否存在: `-d` 或 `!-d`.
        * 检查文件, 目录, 软链接是否存在: `-e` 或 `!-e`.
        * 检查是否为可执行文件: `-x` 或 `!-x`.
    
      
#### SERVER_REWRITE
#### FIND_CONFIG
* location 指令.
    如果多个相同类别的规则都匹配则使用最长的匹配规则.
    匹配规则:
    * 前缀字符串
        * 常规前缀匹配. 优先级最低, 低于正则.
        * `=`: 精确匹配. 优先级最高.
        * `^~` 匹配后不再进行正则匹配. 优先级高于正则匹配.
    * 正则匹配. 按配置中出现的顺序依次匹配.
    * `@` 用于内部跳转的命名 location. 

    [location 匹配优先级](https://files-kyo.oss-cn-hongkong.aliyuncs.com/FuC0A0UOrcQeIMC5UFerDfiSEROi.png).

    
* merge_slashes 指令: 是否将多个 `/` 合并为一个. 默认为 `on`.    
#### REWRITE
* if 指令.
#### POST_REWRITE

### ACCESS
#### PREACCESS
* limit_conn 模块: 限制并发连接数.
    生效范围为全部 worker 进程(基于共享内存). 限制的有效性取决于  key 的设计: 比如 `POST_READ` 阶段 realip 模块取到的真实 IP.

* limit_req 模块: 限制每单位处理请求数.
    生效范围为全部 work 进程(基于共享内存). 使用 leaky bucket 算法实现.
#### ACCESS
此阶段主要用来控制是否有**权限**访问.

* satisfy 指令
    当值为 `all` 时, 必须所有 ACCESS 中的模块全部通过时, 才可以继续访问.
    当值为 `any` 时, 只要任一个 ACCESS 中的模块通过后, 就可以继续访问了.

* access 模块: 用于控制 IP 的访问权限. 提供了 `allow` 和 `deny`  指令.
* auth_basic 模块: 用于进行 HTTP Basic Authutication 协议认证. 提供了 `auth_basic` 和 `auth_basic_user_file` 指令.
* auth_request 模块: 
      向上游服务转发请求, 若上游服务返回的响应码是 2xx, 则继续执行, 若上游服务返回的是 401 或 403, 则将响应返回给客户端.
    其实现是收到请求后, 生成子请求, 通过反向代理技术把请求传递给上游服务.
    提供的指令有 `auth_request`, `auth_request_set`.
    
#### POST_ACCESS

### CONTENT

#### PRECONTENT
* try_files 指令: 依次试图访问多个 url 对应的文件(由 root 或 alias 指令指定), 当文件存在时直接返回文件内容, 如果所有文件都不存在, 则按最后一个 URL 的结果或者 code 返回.
* mirror 指令: 处理请求时, 生成子请求访问其他服务, 对子请求的返回值不做处理.

#### CONTENT
绝大多数 Nginx 模块在向 content 阶段注册配置指令时, 本质上是在当前的 location 配置块中注册所谓的 "内容处理程序(content handler)". **每个 location 只能有一个 "content handler". 因此, 当在 location 中同时使用多个模块的 content 阶段指令时, 只有其中一个模块能成功注册 "content handler"**.
配置指令 `echo_before_body`, `echo_after_body` 可与其他 content 指令结合执行. 因这两个指令运行在 Nginx 的 "输出过滤器" 中, 不属于 Nginx 的 11 个运行阶段.


* root 指令: 将 URL 映射为文件路径, 并返回其内容. 此指令会将 `location` 指令所匹配的路径添加到其值后查找文件.
* alias 指令: 将 URL 映射为文件路径, 并返回其内容. 此指令会忽略 `location` 匹配的路径, 把剩余部分添加到其值后查找文件.
* 变量 `request_filename`: 待访问文件的完整路径.
* 变量 `document_root`: 由 URI 和 `root/alias` 规则生成的文件夹路径.
* 变量 `realpath_root`: 将 `document_root` 中的软链接等转换成真实路径后的值.
* types 指令: 指定返回文件时文件后缀名与返回的 Respone Header 中 `content-type` 值的对应关系.
* Nginx 的 static 模块在进行 `root/alias` 匹配时, 如果发现访问的目标是目录, 而请求 URL 并没有以 `/` 结尾时, 会返回一个到此目录的 301 重定向. 这个返回结果可以使用指令 `server_name_in_redirect`, `port_in_redirect`, `absolute_redirect` 来控制.
* index 指令: 指定做为 index 文件的名称.
* auto_index 指令: 如果 URL 匹配目录时, 尝试返回目录中的文件列表. 相关指令有 `autoindex_exact_size`, `autoindex_format`, `autoindex_localtime`. 注意, `index` 指令是先于 `auto_index` 执行的. 可以将 `index` 的值设为一个不存在的文件从而防止其执行.
* concat 模块: 可以把多个文件的内容合并到一个 HTTP 响应中返回.

#### POSTCONTENT
* ngx_http_sub_filter_module 模块: 用于将响应中指定的字符串替换成新的字符串. 通过 `--with-http_sub_module` 启用. 
* ngx_http_addition_filter_module 模块: 用于在响应前或响应后增加内容. 增加内容的方式是通过新增子请求的响应完成. 通过 `--with-http_addition_module` 启用.
### LOG
##### access 
* log_format 指令: 定义日志格式.
* access_log 指令: 定义日志路径. 路径中可包含变量. 支持缓存, 压缩功能.
* open_log_file_cache 指令: 控制日志缓存功能.
##### error
* log_not_found 指令: 值为 `off` 时不显示文件找不到的错误.


## 指令
### server_name
`server_name` 值是与请求中的 `Host` 头部做匹配, 而不是 URL 中使用的域名.
`server_name` 可使用的值有具体域名, 带 `*` 的泛域名和使用正则表达式(以 `~` 前缀开头)表示的域名值.
`server_name` 指令后可以跟多个域名, 其中第一个为主域名.

```nginx
## 使用正则匹配域名时, 可以使用括号的形式创建变量供后面使用
server_name ~^(www\.)?(.+)$;
root /sites/$2; # 使用 $N 的形式引用 server_name 中正则匹配的变量

server_name ~^(www\.)?(?<domain>.+)$;
root /sites/$domain; # 使用变量的形式引用 server_name 中正则匹配的变量


## 以 . 开头的域名可匹配多种
server_name .kyo.rocks # 可匹配 kyo.rocks *.kyo.rocks

## _ 为默认匹配
server_name _;
# listion 指令中使用 default 值可同时指定其为 default_server
listion 80 default;

## "" 匹配没有传递 Host 头部的请求
server_name "";
```

#### 值匹配顺序

1. 精确域名匹配
2. `*` 在前的泛域名
3. `*` 在后的泛域名
4. 按文件中出现的顺序匹配正则表达式域名

    如果配置中使用如 `include conf.d/*.conf` 这种形式加载多个文件, 其出现顺序是由 linux 中 [glob](http://man7.org/linux/man-pages/man7/glob.7.html) 的匹配规则决定的.
6. default server

### server_name_in_redirect
指定重定向时使用哪个域名. 如果值为 `on` 则使用主域名, 为 `off` 则使用当前匹配的域名. 默认为 `off`.

### error_page
为指定的返回码定义行为.