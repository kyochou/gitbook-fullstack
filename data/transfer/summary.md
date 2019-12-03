# Transfer
## Note
序列化(serialization, marshalling)的过程是指将数据结构或者对象的状态转换成可以存储或传输的格式. 反向操作就是反序列化(deserialization, unmarshalling)的过程.

## Process
两个进程之间如果要通信, 很显然必须要建立一个连接, 通过它来相互传输数据. 

### Socket
Socket(套接字) 是一种操作系统提供的进程间通信机制. 它以五元组(`src_ip, src_port, dest_ip, dest_port, protocol_type`)的形式组成套接字地址进行通信.
