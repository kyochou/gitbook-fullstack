# GoRoutine

## Practices
使用 `select` 对 `chan` 进行控制时, 需注意是否会发生多个 `case` 子句同时被满足的情况.

## Implement
Go 的调试器使用一个叫做 `GOMAXPROCS` 的环境变量来决定会有多少个操作系统的线程同时执行 Go 的代码. 其默认值是运行所在机器的 CPU 核心数.

### goroutines&thread
#### 动态栈
每个 OS 线程都有一个固定大小的内存块(一般是 2MB)来做栈, 这个栈会用来存储当前正在被调用或挂起的函数的内部变量. 相反, 一个 goroutine 会以一个很小的栈开始其生命周期(一般是 2KB), 和 OS 线程不太一样的是这个栈的大小并不是固定的, 栈的大小会根据需要动态地伸缩(最大值是 1GB). 
这种动态栈机制使得创建一个 goroutine 的代价很小, 创建百万级的 goroutine 完全是可行的.
Go 语言拥有自己的调试器, 使得重新调试一个 goroutine 的代价比调度一个线程代价要低得多.

## Note
Don't communicate by sharing memory; share memory by communicating.(不要通过共享内存来通信, 而应该通过通信来共享内存). 这是 Go 并发编程最重要的编程理念.
Go 并发编程模型中的三个主要元素是: G(goroutine), P(processor), M(machine). 其中 M 指代的是系统级线程(OSThread); P 指的是一种可以承载若干个 G, 且能够使这些 G 适时地与 M 进行对接, 并得到真正运行的中介.

Go 语言提供了基于 CSP(Communicating Sequential Processes, 顺序进程通信) 的并发特性支持. 
在 Go 语言中, 每个并发的执行单元叫作一个 goroutine(go 程).
与一个进程总会有一个主线程类似, 每个独立的 Go 程序在运行时也总会有一个 main goroutine.
当一个程序启动时, 其主函数在一个单独的 goroutine 中运行, 即 main goroutine. 新的 goroutine 使用 go 语句创建. go 语句会使其后的函数在一个启用(不一定是新创建)的 goroutine 中运行, 而 go 语句本身会迅速地完成.
当主函数返回时, 所有的 goroutine 都会直接打断, 程序退出. 除了从主函数退出或者直接退出程序外, 没有其他编程方法能够让一个 Goroutine 来打断另一个. 但我们可通过 Goroutine 之间的通信来让一个 Goroutine 请求其它的 Goroutine, 并让其自己结束执行.
对于语句 `go f(x, y, z)`, `x`, `y`, `z` 的求值发生在当前的 goroutine 中, 而 `f` 的执行则发生在新的 goroutine 中.
goroutine 没有可以被程序员获取到 ID 的概念. 这一点是设计上故意而为之, 防止 thread-local storage 被滥用.
生产线的隐喻对于理解 channels 和 goroutines 的工作机制是很有帮助的.

## Refs
* [来，控制一下 goroutine 的并发数量](https://github.com/EDDYCJY/blog/blob/master/talk/control-goroutine.md)


