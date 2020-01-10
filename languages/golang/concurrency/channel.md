
# Channels
一个 channels 是一个通信机制, 它可以让一个 goroutine 通过它给另一个 goroutine 发送值信息.
每个 channel 都有一个特殊的类型, 也就是 channels 可以发送的数据类型.
两个类型相同的 channel 可以进行比较操作.
channel 的零值为 nil. 对于值为 nil 的 channel, 对它的发送和接收操作都会永久地阻塞.

```go
ch := make(chan int) // 创建 int 类型的 channel
ch <- x // 发送信息
x = <-ch // 接收信息
<-ch // 接收信息, 结果忽略
close(ch) // 关闭 channel
```

go 语言的 range 循环可以直接在 channel 上迭代. 当 channel 被关闭并且没有值时跳出循环.

和垃圾变量不同, 泄漏的 goroutines (发送了 chan 而没有被接收)并不会被自动回收, 因此确保每个不再需要的 goroutine 能正常退出是重要的. 特别在使用不带缓存的 channels 时尤要注意.
对 channels 的读写是并发安全的.

## 不带缓存的 channels
一个基于无缓存的 channel 的发送/接收操作都会导致当前 goroutine 的阻塞, 直到另一个 goroutine 执行了接收/发送操作. 也就是说基于无缓存 channel 的发送和接收操作将导致两个 goroutine 做一次同步操作. 因为这个原因, 无缓存的 channels 也被称为同步 channels.
在讨论并发编程时, 当我们说 x 事件在 y 事件之前发生(happens before), 我们并不是在强调 x 事件在时间上比 y 时间更早; 我们要表达的意思是要保证在此之前的事件都已经完成了, 例如在此之前的更新某些变量的操作已经完成, 你可以放心依赖这些已完成的事件了.
当我们说 x 事件既不是在 y 事件之前发生也不是在 y 事件之后发生, 我们就说 x 事件和 y 事件是并发的. 这并不是意味着 x 事件和 y 事件就一定是同时发生的, 我们只是不能确定这两个事件发生的先后顺序.
**当通过一个无缓存 channel 发送数据时, 接收者收到数据 发生在 唤醒发送者 之前(happens before)**.

## 串联的 Channels(Pipeline)
Channels 也可以用于将多个 goroutine 链接在一起, 一个 channel 的输出作为下一个 channel 的输入. 这种串联的 channels 就是所谓的管道(pipeline).

## 单方向的 Channels
类型 `chan<- int` 表示一个只发送 int 的 channel, 只能发送不能接收; 类型 `<-chan int` 表示一个只接收 int 的 channel, 只能接收不能发送. 这种限制将在编译期检测.

## 带缓存的 Channels
带缓存的 channel 内部持有一个元素队列. 队列的最大容量是在调用 `make` 函数创建 channel 时通过第二个参数指定的.
向缓存 channel 的发送操作就是向内部缓存队列的尾部插入元素, 接收操作则是从队列的头部删除元素. 如果内部缓存队列是满的, 那么发送操作将阻塞直到因另一个 goroutine 执行接收操作而释放了新的队列空间. 相反, 如果 channel 是空的, 接收操作将阻塞直到另一个 goroutine 执行发送操作而向队列插入元素.  
使用 `cap` 函数获取 channel 内部缓存的容量, 使用 `len` 函数获取 channel 内部元素的个数.

## Channel 的关闭
关闭一个 Channel 意味着不能再向这个通道发送值了. 这个特性可以用来给这个 Channel 的接收方传达工作已经完成的信息.
使用 `close` 函数可以关闭一个指定的 `chan`, channel 的接收操作支持第二个参数, 用来确定 channel 已经关闭并且其中没有未接收的值.
一个已关闭的 channel, 对其执行发送操作将导致 panic 异常;  对其执行接收操作依然可以接收到之前已经成功发送的数据, 如果 channel 中已经没有数据, 后续的接收操作将不再阻塞, 并立即返回一个零值的数据. 
channel 并不需要显式关闭, 只有当需要告诉 goroutine 所有数据都发送完毕时才需要显式关闭.
channel 只能由发送方关闭. 不要在接收端关闭 channel.
一个 Channel 有多个接收者时, close Channel 会唤醒所有接收者.

```go

func main() {
	done := make(chan bool)
	jobs := make(chan int)

	go func() {
		for {
			job, ok := <-jobs // ok 为 false 表示通道被关闭
			if ok {
				fmt.Println(`job: `, job)
			} else {
				fmt.Println(`done`)
				break
			}
		}
		done <- true
	}()
	
	jobs <- 1
	jobs <- 2
	close(jobs)
	<-done
}


```

## select 语句
使用 select 语句可以同时处理多个通道.
select 语句会一直阻塞, 直到有一个条件被执行.
很多时候, 我们需要判断通道是否被关闭, 以执行相关操作.
对于每个 case 语句, 都至少要包含一个代表发送操作或接收操作的表达式.
仅当 select 语句中的所有 case 表达式都被求值完毕后, 它才会开始选择候选(要执行的)分支. 如果有多个候选分支满足条件, 那么它会用一种伪随机算法选择其中的一个执行. (这并不会造成通道数据的丢失, case 语句应该是在真正执行时才去通道取出数据)

```go
select {
    case <-chan1:
      // ...
    case val, ok := <-chan2: // 自通道读取值
      // ...
    default:
    // ...
}
```

## Refs
* [Go语言channel备忘录](https://pengrl.com/p/23102/)
* [Go语言的有缓冲channel和无缓冲channel](https://pengrl.com/p/21027/)