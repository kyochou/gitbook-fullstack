# I/O


##  Linux IO 模型
普通输入操作的步骤:
* 等待数据准备好
* 从内核向进程复制数据

网络数据输入包含的步骤:
* 等待数据从网络送达, 到达后被复制到内核缓冲区
* 把数据从内核缓冲区复制到应用程序缓冲区

### 同步
#### 阻塞式 IO
使用系统调用, 并一直阻塞直到内核将数据准备好, 之后再由内核缓冲区复制到用户态, 在等待内核准备的这段时间什么也干不了.

![阻塞调用](https://files-kyo.oss-cn-hongkong.aliyuncs.com/FmHIPc92TFkGPQ2vRbr8JeKTrwlF.png)

#### 非阻塞式 IO
内核在没有准备好数据的时候会返回错误码, 而调用程序不会休眠, 而是不断轮询询问内核数据是否准备好.

![非阻塞调用](https://files-kyo.oss-cn-hongkong.aliyuncs.com/FuvJGAGvzdysj6xNy061WxQWflyD.png)

##### IO 多路复用
类似于非阻塞, 只不过轮询不是由用户线程去执行, 而是由内核去轮询.
IO 多路复用至少有两次系统调用, 如果只有一个代理对象, 性能不如非阻塞式 IO, 但是由于它可以同时监听很多套接字, 所以性能会比较好.

多路复用包括:
* select: 线性扫描所有监听的文件描述符, 不管他们是不是活跃的. 有最大数量限制.
* poll: 同 select, 不过数据结构不同, 需要分配一个 pollfd 结构数组维护在内核中. 它没有大小限制, 不过需要很多复制操作.
* epoll: 用于代替 poll 和 select, 没有大小限制. 使用一个文件描述符管理多个文件描述符, 使用红黑树存储. 同时用事件驱动代替了轮询. 

##### 信号驱动式 IO
使用信号代替轮询, 内核在数据准备就绪时通过信号来进行通知.

### 异步
异步 IO 依赖信号处理程序来进行通知. 与前面 IO 模型不同的时: 前面的都是数据准备阶段的阻塞与非阻塞, 异步 IO 模型通知的是 IO 操作已经完成, 而不是数据准备完成.

![模型对比](https://user-gold-cdn.xitu.io/2018/10/30/166c578ad18a1d40)

`select`, `poll` 属于 IO 复用模型.
Linux 的 `epoll` 和 Unix 的 `kqueue` 也是属于 IO 复用模型, 并且添加了基于 callback 的回调机制.
Windows 的 `iocp` 是异步 IO 模型.

### Refs
* [五种 IO 模型介绍和对比](https://juejin.im/post/5bd32b84f265da0ac962e7c9)



## Java IO 模型
### BIO
即 `java.io` 包.
它是基于流模型实现的, 交互的方式是同步, 阻塞方式.
其中:
* InputStream, OutputStream 基于字节操作 IO.
* Writer, Reader 基于字符操作 IO.
* File 基于磁盘操作 IO.
* Socket 基于网络操作 IO.

### NIO
Java1.4 引入了 `java.nio` 包.
提供了 Channel, Selector, Buffer 等新的抽象, 可以构建多路复用的, 同步非阻塞 IO 程序.

### AIO
Java1.7 引入了 AIO(Asynchronous IO).
提供了异步非堵塞的 IO 操作方式. AIO 是基于事件和回调机制实现的.

```java
// java.nio.Files
// 写入文件（追加方式：StandardOpenOption.APPEND）
Files.write(Paths.get(filePath), Content.getBytes(StandardCharsets.UTF_8), StandardOpenOption.APPEND);

// 读取文件
byte[] data = Files.readAllBytes(Paths.get(filePath));
System.out.println(new String(data, StandardCharsets.UTF_8));

```

### Refs
* [Java 核心（五）深入理解 BIO、NIO、AIO](https://juejin.im/post/5c048e0ff265da613a53c572)


## IO 模型比较
* 同步阻塞: IO 性能一般很差, CPU 大部分在空闲状态.
* 同步非阻塞: 提升 IO 性能的常用手段, 就是将 IO 的阻塞改为非阻塞方式, 尤其在网络 IO 是长连接, 同时传输数据也不是很多的情况下, 提升性能非常有效. 这种方式通常能提升 IO 性能, 但是会增加 CPU 消耗, 要考虑系统的瓶颈是在 IO 还是 CPU 上.
* 异步阻塞: 这种方式在分布式数据库中经常用到. 异步阻塞对网络 IO 能够提升效率, 尤其是同时写多份相同数据的情况.
* 异步非阻塞: 这种组合方式用起来比较复杂, 只有在一些非常复杂的分布式情况下使用, 如集群之间的消息同步. 它适合同时要传多份相同的数据到集群中不同的机器, 同时数据的传输量虽然不大, 但是却非常频繁. 这种网络 IO 用这个方式性能能达到最高.


## Refs
* [BIO，NIO，AIO 概览](https://pjmike.github.io/2018/08/24/BIO%EF%BC%8CNIO%EF%BC%8CAIO%E6%A6%82%E8%A7%88/)
* [各种 IO 复用模式之 select，poll，epoll，kqueue，iocp 分析](https://cloud.tencent.com/developer/article/1373483)
* [再谈 select, iocp, epoll,kqueue 及各种 I/O 复用机制](https://blog.csdn.net/shallwake/article/details/5265287)