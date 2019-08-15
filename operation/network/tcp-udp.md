# TCP/UDP
## Port

tcp/udp 一般采用**五元组**来定位一个连接:
```
src_ip, src_port, dest_ip, dest_port, protocol_type
```
一个端口是可以建立多个连接的, 只要五元组中任意一个元素不一样, 那就可以认为是一个不同的连接.

可以通过多网卡或虚拟网卡来解决端口数目的限制.

#### 参考
* [理解端口复用（四元组）](https://blog.csdn.net/dlf1769/article/details/78786775)
* [突破 netty 单机最大连接数](https://www.jianshu.com/p/490e2981545c)

## TCP

## UDP
因 UDP 的 "send-and-forget" 特性, 除非在服务端明确给出回应(可使用 nc 命令起一个 server), 否则无法测试一个 UDP 端口是否已打开.
udp 本身特点决定了它非常适合如日志, 监控等数据收集类场景, 即在不影响程序性能的基础上提升程序的可监测性(Measurability).