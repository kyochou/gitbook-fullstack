# NGX 模块

## Request
### Body
由于 Nginx 是为了解决负载均衡场景延生的, 所以它默认是不读取 body 的. 根据需要正确读取, 丢弃 body 对 OpenResty 开发至关重要.

## Respone
* ngx.eof: 立即关闭连接, 把数据返回给终端.