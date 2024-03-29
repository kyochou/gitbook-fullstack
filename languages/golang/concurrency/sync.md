# 同步与锁  
概括来讲, 同步的用途有两个, 一个是避免多个线程在同一时刻操作同一个数据块, 另一个是协调多个线程, 以避免它们在同一时刻执行同一个代码块.  
我们所说的同步其实就是在控制多个线程对共享资源的访问. 一个线程在想要访问某一个共享资源的时候, 需要先申请对该资源的访问权限, 并且只有在申请成功之后, 访问才能真正开始. 而当线程对共享资源的访问结束时, 它还必须归还对该资源的访问权限, 若要再次访问仍需申请.  
锁的主要作用就是保证多个 Goroutine 在访问同一片内存时不会出现混乱的问题. 锁其实是一种并发编程中的同步原语(Synchronization Primitives).  
自旋(Spinnig)是在多线程同步的过程中使用的一种机制, 当前的进程在进入自旋的过程中会一直保持 CPU 的占用, 持续检查某个条件是否为真. 在多核 CPU 上, 自旋的优点是避免了 Goroutine 的切换. 所以如果使用恰当会对性能带来非常大的增益.  
  
  
## sync  
### Mutex(互斥锁)  
sync 包的 Mutex 类型支持 Lock 和 Unlock 操作. 用来保证他们之间的代码同一时间只有一个 goroutine 能访问.  
我们总应该保证, 对于每一个锁定操作, 都要有且只有一个对应的解锁操作.  
惯例来说, 被 Mutex 所保护的变量的声明应紧随 Mutex 的声明之后.  
使用 `defer` 来调用 `Unlock` 方法, 以防忘记了解锁操作. `defer` 调用只会比显式地调用 `Unlock` 成本高那么一点点, 不过却在很大程度上保证了代码的整洁性. **对于大多数并发程序来说, 代码的整洁性比过度的优化更重要**.  
`sync.Mutex` 类型是一个结构体, 属于值类型中的一种. 因此, 当你使用 `sync.Mutex` 时, 确保 `sync.Mutex` 和其保护的变量没有被导出(也不要在函数间传递或赋值给其他变量).  
饥饿模式的主要功能是保证互斥锁获取的 "公平性(Fairness)".  
  
### RWMutex(读写互斥锁)  
`sync.RWMutex` 锁允许多个只读操作并行执行(`RLock`, `RUnlock`, 但写操作会完全互斥. 这种锁叫做 "多读单写" 锁.  
相比于互斥锁(`sync.Mutex`), 读写锁可以实现更加细腻的访问控制.  
对于某个受到读写锁保护的共享资源, 多个写操作不能同时进行, 写操作和读操作也不能同时进行, 但多个读操作却可以同时进行.  
  
### WaitGroup  
用于等待所有 goroutines 都完成后再进行后续操作. 这个函数可以让我们很方便的实现一对多的 goroutine 协作流程, 即: 一个分发子任务的 goroutine, 和多个执行子任务的 goroutine, 共同来完成一个较大的任务.  
`sync.WaitGroup` 类型有三个指针方法: Add, Done 和 Wait.  
不要把 Add 操作和 Wait 操作放在不同的 goroutine 中执行. 使用 "先统一 Add, 再并发 Done, 最后 Wait" 这种标准方式.  
`sync.WaitGroup` 的值是可以被复用的, 但需要保证其计数周期的完整性(Add 后要再次归零).  
  
### Once  
sync.Once 可以保证其保护的代码可以安全的仅执行一次. 一般用于执行只应该执行一次的任务. 比如初始化连接池.  
只要传入某个 Do 方法的参数函数没有结束执行, 任何之后调用该方法的 goroutine 就都会被阻塞. 只有在这个参数函数执行结束以后, 那些 goroutine 才会逐一被唤醒.  
  
### Cond  
Go 标准库提供 Cond 原语的目的是, 为 "等待/通知" 场景下的并发问题提供支持.
与利用 channel 相比, Cond 类型的性能要高很多.
`sync.Cond` 并不是用来保护临界区和共享资源的, 它是用于协调想要访问共享资源的那些线程的. 当共享资源的状态发生变化时, 它可以被用来通知被互斥锁阻塞的线程.  
`sync.Cond` 的最大优势就是在效率方面的提升. 当共享资源的状态不满足条件的时候, 想操作它的线程再也不用循环往复地(`for{}` 循环)做检查了, 只要等待通知就好了.  
`sync.Cond` 提供了三个方法: 等待通知(wait), 单发通知(signal)和广播通知(broadcast).  
我们需要利用 `sync.NewCond` 函数创建 `sync.Cond` 的指针值. 这个函数需要一个 `sync.Locker` 类型的参数值.  
利用 `sync.Cond` 可以实现单向的通知, 而双向的通知则需要两个条件变量.  
在包裹 `sync.Cond.Wait` 方法时, 明确检查是否应该使用 `for` 语句.  
`sync.Cond.Signal` 方法和 `sync.Cond.Broadcast` 方法都是被用来发送通知的, 不同的是, 前者的通知只会唤醒一个因此而等待的 goroutine, 而后者的通知却会唤醒所有为此等待的 goroutine.   
`sync.Cond.Wait` 方法总会把当前的 goroutine 添加到通知队列的队尾, 而 `sync.Cond.Signal` 方法总会从通知队列的队首开始查找可被唤醒的 goroutine.  
如果你确定只有一个 goroutine 在等待通知, 或者只需要唤醒任意一个 goroutine 就可以满足要求, 那么使用 `sync.Cond.Signal` 方法就好了. 否则, 使用 `sync.Cond.Broadcast` 方法总没错.  
与 `Wait` 方法不同, `Signal` 和 `Broadcast` 方法并不需要在互斥锁的保护下执行.   

#### Wait
调用 `Wait()` 不只是阻塞, 它会挂起当前的协程, 允许其他协程在 OS 线程上运行.
  
### Map  
与单纯使用原生 map 和互斥锁的方案相比, 使用 `sync.Map` 可以显著地减少锁的争用. `sync.Map` 本身虽然也用到了锁, 但是, 它其实在尽可能地避免使用锁.  
`sync.Map` 在读多写少时性能会比较好. 而 `concurrent-map` 在 key 的 hash 度高的情况下性能比较好. 在无法确定读写比的情况下, 建议使用 `concurrent-map`.
  
#### Refs  
* [Go 1.9 sync.Map揭秘](https://colobu.com/2017/07/11/dive-into-sync-Map/)  
* [orcaman/concurrent-map](https://github.com/orcaman/concurrent-map/blob/master/README-zh.md)  
* [concurrent-map 和 sync.Map，我该选择哪个？](https://www.cnblogs.com/yjf512/p/17139605.html)
  
  
## x/sync  
### errgroup  
`errgroup.Group` 为我们提供了多任务并行处理功能(对 `sync.WaitGroup` 的封装), 如果有任务发生错误则返回第一个发生的错误. 需要注意的是, 即使有错误发生, `errgroup.Group` 还是会等待所有任务都执行结束后才返回(`Wait`).  
  
### semaphore(信号量)  
`semaphore` 会保证持有的计数器在 0 到初始化的权重(数量)之间. 每次获取资源时都会将 `semaphore` 中的计数器减去对应的数值, 在释放时重新加回来, 当遇到计数器大于信号量大小时就会进入休眠等待其他进程释放信号. 常常会在控制访问资源的进程数量时用到.  
`semaphore` 包对外提供了四个方法:  
* `NewWeighted`: 创建新的信号量;  
* `Acquire`: 获取指定权重的资源, 如果当前没有 "空闲资源", 就会陷入休眠等待;  
* `TryAcquire`: 获取指定权重的资源, 但是如果当前没有 "空闲资源", 就会直接返回 `false`;  
* `Release`: 释放指定权重的资源. 资源释放后会按 FIFO 的顺序唤醒等待信号量的 Goroutine.  
  
### singleflight  
`singleflight` 能够在一个服务中抑制对下游的多次重复请求. 它的主要作用就是对同一个 `key` 最终只会进行一次函数调用.  
`singleflight.Group` 提供了以下接口:  
* `Do` 和 `DoChan` 一个用于同步阻塞调用传入的函数, 一个用于异步调用传入的参数并通过 Channel 接收函数的返回值. 一旦调用的函数返回了错误, 所有在等待的 Goroutine 也都会接收到同样的错误;  
* `Forget` 方法可以通知 `singleflight` 在持有的映射表中删除某个键, 接下来对该键的调用就会直接执行方法而不是返回缓存的值.  
  
  
## Refs  
* [同步原语与锁](https://draveness.me/golang/docs/part3-runtime/ch06-concurrency/golang-sync-primitives/)  
  
  
