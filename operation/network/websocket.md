# WebSocket

WebSocket 是 HTML5 提供的一种浏览器与服务器进行全双工通讯的网络技术, 属于应用层协议. 它基于 TCP 传输协议, 并复用 HTTP 的握手通道. 其最大的特点就是服务器可以主动向客户端推送信息, 客户端也可以主动向服务器发送信息, 是真正的双向平等对话.
其他特点包括:
* 建立在 TCP 协议之上.
* 与 HTTP 协议有着良好的兼容性. 默认端口也是 80 和 443, 并且握手阶段采用 HTTP 协议, 因此握手不容易被屏蔽, 能穿透各种 HTTP 代理服务器.
* 数据格式比较轻量, 性能开销小, 通信高效.
* 可以发送文本, 也可以发送二进制数据.
* 没有同源限制, 客户端可以与任意服务器通信.
* 协议标识符为 `ws`, `wss`, 服务器地址就是 URL.

HTTP 和 WebSocket 都支持配置证书(`wss://`).
![http vs websocket](http://www.ruanyifeng.com/blogimg/asset/2017/bg2017051503.jpg)

TCP 连接的维护并不是一个耗资源的事情, 只是占用一个句柄而已, 没有消息传输基本不怎么费资源. 真正有压力的是连接上消息的下推.
服务器的单机连接数一般会控制在 100w 以内.

决定手头的工作是否需要使用 WebSocket 技术的方法很简单:
* 你的应用提供多个用户相互交流吗?
* 你的应用是展示服务器端经常变动的数据吗?

## 数据帧
WebSocket 客户端, 服务端通信的最小单位是帧(frame), 由 1 个或多个帧组成一条完整的消息(message):
1. 发送端: 将消息切割成多个帧, 并发送给服务端.
2. 接收端: 接收消息帧, 并将关联的帧重新组装成完整的消息.



## Refs
* [WebSocket](https://zh.wikipedia.org/wiki/WebSocket)
* [WebSocket 教程](http://www.ruanyifeng.com/blog/2017/05/websocket.html)
* [WebSocket协议：5分钟从入门到精通](https://www.cnblogs.com/chyingp/p/websocket-deep-in.html)