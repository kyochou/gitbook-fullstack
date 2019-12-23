# TCP

## Nagle 算法
Nagle 算法的目的是减少发送小包的数量, 从而减小带宽, 并提高网络吞吐量, 付出的代价是有时会增加服务的延时. 引入的延时通常在毫秒级别.
Nagle 算法所对应的 TCP socket 选项是 `TCP_NODELAY`. 开启 `TCP_NODELAY` 可以禁用 Nagle 算法. 禁用 Nagle 算法后, 数据将尽可能快的被发送出去.
另外, 我们也可以在应用层对数据进行缓存合并发送来达到 Nagle 算法的目的. 在 Go 语言中即使用 `bufio.Writer`. 使用 `bufio.Writer` 还有一个好处是减少了调用 `write` 系统调用的次数, 但相应的, 增加了数据拷贝的开销.
在 Go 语言中, `TCP_NODELAY` 默认是开启的, 可通过 `func (c *TCPConn) SetNoDelay(noDelay bool) error` 方法控制它.

### Refs
* 