# Socket

`netstat` 和 `ss` 命令的输出类似, 都展示了套接字的状态(State), 接收队列(Recv-Q), 发送队列(Send-Q), 本地地址, 远端地址, 进程 PID 和进程名称等).
当 Socket 处于连接状态(Established)时, Recv-Q 表示 Socket 缓冲还没有被应用程序取走的字节数(即接收队列长度), 而 Send-Q 表示还没有被远端主机确认的字节数(即发送队列长度).
当 Socket 处于监听状态(Listening)时, Recv-Q 表示 syn_backlog 的当前值, 而 Send-Q 表示最大的 syn_backlog 值.
syn_backlog 是 TCP 协议栈中的半连接队列长度. 所谓半连接, 就是还没有完成 TCP 三次握手的连接. 服务器收到客户端的 SYN 包后, 就会把这个连接放在半连接队列中, 然后再向客户端发送 SYN+ACK 包.
