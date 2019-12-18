# Socket

## Linux 内核参数
### 连接队列
TCP 建立连接时要经过 3 次握手. 在客户端向服务器发起连接时, 对于服务器而言, 一个完整的连接建立过程, 服务器会经历 2 种状态: SYN_REVD, ESTABLISHED. 对应也会维护两个队列: 一个存放 SYN 的队列(半连接队列); 一个存放已经完成连接的队列(全连接队列). 应用程序只会从已完成连接的队列中获取请求.
这两个队列一旦满后, 很容易出现丢包, 或连接被复位. 所以, 如果服务器并发访问量高, 这两个队列的设置就非常重要了.
`syn_backlog` 是 TCP 协议栈中的半连接队列长度. 所谓半连接, 就是还没有完成 TCP 三次握手的连接. 服务器收到客户端的 `SYN` 包后, 就会把这个连接放在半连接队列中, 然后再向客户端发送 `SYN+ACK` 包.
对于 Linux 而言, 基本上任意语言实现的通信框架或服务器程序在构造 socket server 时, 都提供了 `backlog` 参数. 因为在监听端口时, 都会调用系统底层 API: `int listen(int sockfd, int backlog)`. `backlog` 参数描述的是服务器端 TCP ESTABLISHED 状态对应的全连接队列长度.
内核参数 `net.core.somaxconn` 定义了系统级别的全连接队列的最大长度. 所以 `全连接队列长度 = min(backlog, net.core.somaxconn)`.
半连接队列长度由内核参数 `tcp_max_syn_backlog` 决定, 不过当使用 SYN Cooike(就是内核参数 `net.ipv4.tcp_syncookies = 1`)时, 这个参数无效. 即 `半连接队列长度 = min(backlog, net.core.somaxconn, tcp_max_syn_backlog)`.

#### Refs
* [理解 Linux backlog/somaxconn 内核参数](https://jaminzhang.github.io/linux/understand-Linux-backlog-and-somaxconn-kernel-arguments/)



`netstat` 和 `ss` 命令的输出类似, 都展示了套接字的状态(State), 接收队列(Recv-Q), 发送队列(Send-Q), 本地地址, 远端地址, 进程 PID 和进程名称等).
当 Socket 处于连接状态(Established)时, Recv-Q 表示 Socket 缓冲还没有被应用程序取走的字节数(即接收队列长度), 而 Send-Q 表示还没有被远端主机确认的字节数(即发送队列长度).
当 Socket 处于监听状态(Listening)时, Recv-Q 表示 `syn_backlog` 的当前值, 而 Send-Q 表示最大的 `syn_backlog` 值.

