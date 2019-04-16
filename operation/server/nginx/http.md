# HTTP

## HTTP 请求处理的 11 个阶段

Nginx 通过变量在模块间传递数据.

### POST_READ
Nginx 读取完所有的请求头部之后, 没有做任何再加工前.

* realip 模块

    `X-Real-IP` 用于传递用户 IP.
    `X-Forwarded-For` 用于传递代理 IP.

### REWRITE
#### SERVER_REWRITE
#### FIND_CONFIG
#### REWRITE
#### POST_REWRITE

### ACCESS
#### PREACCESS
#### ACCESS
此阶段主要用来控制是否有**权限**访问.
#### POST_ACCESS

### CONTENT
#### PRECONTENT
#### CONTENT
####LOG



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