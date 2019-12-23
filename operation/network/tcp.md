# TCP
## Options
### TCP_KEEPALIVE
`TCP_KEEPALIVE` 发送包含空的(或很小)的包体负载的 TCP 报文给对端, 并且对端会回复 keepalive ACK 确认包. 它有三个主要参数:


### TCP_QUICKACK
TCP 采用两种方式来发送 ACK: 快速确认和延迟确认:
* 在快速确认模式中, 本端接收到数据包后, 会立即发送 ACK 给对端.
* 在延迟确认模式中, 本端接收到数据包后, 不会立即发送 ACK 给对端, 而是等待一段时间, 如果在此期间:
    * 本端有数据包要发送给对端. 就在发送数据包的时候捎带上此 ACK, 如此一来就节省了一个报文.
    * 本端没有数据包要发送给对端. 延迟确认定时器会超时, 然后发送纯 ACK 给对端.

#### Refs
* [Linux下TCP延迟确认(Delay ACK)机制](https://blog.tms.im/2017/05/15/delay-ack)

### TCP_NODELAY(Nagle 算法)
Nagle 算法的目的是减少发送小包的数量, 从而减小带宽, 并提高网络吞吐量, 付出的代价是有时会增加服务的延时. 引入的延时通常在毫秒级别.
Nagle 算法所对应的 TCP socket 选项是 `TCP_NODELAY`. 开启 `TCP_NODELAY` 可以禁用 Nagle 算法. 禁用 Nagle 算法后, 数据将尽可能快的被发送出去.
另外, 我们也可以在应用层对数据进行缓存合并发送来达到 Nagle 算法的目的. 在 Go 语言中即使用 `bufio.Writer`. 使用 `bufio.Writer` 还有一个好处是减少了调用 `write` 系统调用的次数, 但相应的, 增加了数据拷贝的开销.
在 Go 语言中, `TCP_NODELAY` 默认是开启的, 可通过 `func (c *TCPConn) SetNoDelay(noDelay bool) error` 方法控制它.

#### Refs
* [Go 语言使用 TCP_NODELAY 控制发包流量](https://mp.weixin.qq.com/s?__biz=MzAxMTA4Njc0OQ==&mid=2651438085&idx=2&sn=290d6b7a6cbbe7877adc738c13eca6ea)