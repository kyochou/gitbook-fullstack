# Proxy

[HTTP 反向代理流程](https://files-kyo.oss-cn-hongkong.aliyuncs.com/Fvvj3j83vwW4MTs87xHg2SEm9m6U.png)

## HTTP
### http_proxy 模块
对上游服务使用 http/https 协议进行反向代理.
使用 `proxy_pass` 指令开启.
参数 URL 规则:



## Upstream
### Variables
 * upstream_addr: 上游服务器的地址.
 * upstream_connect_time: 与上游服务建立连接消耗的时间.
 * upstream_header_time: 接收上游服务发回响应中的 http 头部所消耗的时间.
 * upstream_respone_time: 接收完整的上游服务响应所消耗的时间.
 * upstream_http_名称: 从上游服务返回的响应头部值.
 * upstream_bytes_received: 从上游服务接收到的响应大小.
 * upstream_respone_length: 上游服务返回的 http body 大小.
 * upstream_status: 上游服务返回的 HTTP 状态码. 如未连接上, 则为 502.
 * upstream_cookie_名称: 从上游服务响应头 Set-Cookie 中取出的 cookie 值.
 * upstream_trailer_名称: 从上游服务响应尾部取到的值.

### 负载均衡



#### http_upstream_zone 模块
负载均衡算法默认只在当前 worker 进程中生效. 此模块通过使用共享内存使负载均衡策略对所有 worker 进程生效.
使用指令 `zone` 开启.


#### 加权 Round-Robin 算法
使用加权轮询的方式访问 upstream 中定义的 server 服务.
相关参数有:
* weight: 服务访问权重, 默认为 1.
* max_conns: server 的最大并发连接数. 仅作用于单 work 进程. 默认为 0, 表示不限制.
* max_fails: 在 `fail_timeout` 时间内的最大失败次数.
* fail_timeout: 单位为秒, 默认值为 10. 指定在到达 `max_fails` 次数后, 该 server 被禁用的时间.


#### Hash 算法
通过指定关键字作为 hash key, 基于 hash 算法映射到特定的上游服务器中.
通过添加参数 `consistent` 实现一致性 Hash 算法. 从而避免 server 发生变动时 Hash 算法映射的结果大幅变动.
基于 http_upstream_hash 模块, 使用 `hash` 指令开启.

#### IP Hash 算法
以客户端的 IP 地址作为 hash 算法的关键字, 映射到特定的上游服务器中.
基于 http_upstream_ip_hash 模块, 使用 `ip_hash` 指令开启.

#### 优先选择连接数最少服务算法
从所有上游服务器中, 找出当前并发连接数最少的一个, 将请求转发到它. 如果有多个相同最少连接数的服务器, 则对它们使用 round-robin 算法.
基于 http_upstream_least_conn 模块, 使用 `least_conn` 指令开启.

### 指令

* upstream: 
* server: 指定上游服务器的地址, 可以是域名, IP 地址或 Unix Socket 地址. 相关参数有:
    * backup: 指定当前 server 为备用服务器. 仅当非备份 server 不可用时, 请求才会转发到该 server.
    * down: 标识某台 server 已经下线, 不再提供服务.

    
### FAQs
* 对上游服务使用 `keepalive` 长连接:
    相关指令: `keepalive`, `keepalive_requests`, `keepalive_timeout`.
    
    ```nginx
    upstream servers {
        server 127.0.0.1:8011;
        server 127.0.0.1:8012;
        keepalive 32;
    }
    
    server {
        location / {
            proxy_pass http://servers
            proxy_http_version 1.1;
            proxy_set_header Connection "";
        }
    }
    
    ```