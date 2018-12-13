# Thread

## ReadNote
* [为什么 Goroutine 能有上百万个, Java 线程却只能有上千个?](https://juejin.im/entry/5b48190b5188251af121d7a4)

操作系统线程:
* CPU 的每个核心只能真正并发同时执行一个操作系统线程.
* 线程需要一定的内存用于保存其操作系统相关的信息.

任何将语言线程和操作系统线程进行 1:1 映射的解决方案都无法支持大规模的并发.

JVM 使用的是操作系统的线程.
Goroutine 是由 Go 语言实现的语言级别的调度线程(user space thread), 而不是 内核/OS 所调度的线程.


    