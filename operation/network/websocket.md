# WebSocket

WebSocket 最大的特点就是服务器可以主动向客户端推送信息, 客户端也可以主动向服务器发送信息, 是真正的双向平等对话.
其他特点包括:
* 建立在 TCP 协议之上.
* 与 HTTP 协议有着良好的兼容性. 默认端口也是 80 和 443, 并且握手阶段采用 HTTP 协议, 因此握手不容易被屏蔽, 能穿透各种 HTTP 代理服务器.
* 数据格式比较轻量, 性能开销小, 通信高效.
* 可以发送文本, 也可以发送二进制数据.
* 没有同源限制, 客户端可以与任意服务器通信.
* 协议标识符为 `ws`, `wss`, 服务器地址就是 URL.

HTTP 和 WebSocket 都支持配置证书(`wss://`).
![http vs websocket](http://www.ruanyifeng.com/blogimg/asset/2017/bg2017051503.jpg)


客户端发送心跳包的间隔时长在国内一般不超过 5 分钟, 微信用的是 4 分半. 以前有大厂测试过国内各家运营商的 NAT 超时情况, 有的运营商在 2G/3G 网络下 NAT 超时时长是 5 分钟.
TCP 连接的维护并不是一个耗资源的事情, 只是占用一个句柄而已, 没有消息传输基本不怎么费资源. 真正有压力的是连接上消息的下推.
服务器的单机连接数一般会控制在 100w 以内.


## Refs
* [WebSocket](https://zh.wikipedia.org/wiki/WebSocket)
* [WebSocket 教程](http://www.ruanyifeng.com/blog/2017/05/websocket.html)