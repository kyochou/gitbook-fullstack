# GoRoutine

## Practices
使用 `select` 对 `chan` 进行控制时, 需注意是否会发生多个 `case` 子句同时被满足的情况.

## Sync

## Context
`Context` 的主要作用就是在不同的 goroutine 之间同步请求特定的数据, 取消信号以及处理请求的截止日期.
控制并发有两种经典的方式, 一种是 `sync.WaitGroup`, 另一种就是 `Context`.
有这样一种场景, 一个网络请求 Request, 每个 Request 都需要开启一个 goroutine 做一些事情, 这些 goroutine 又可能会开启其他的 goroutine. 所以我们需要一种可以跟踪 goroutine 的方案, 才可以达到控制他们的目的, 这就是 Go 语言为我们提供的 `Context`, 它就是 goroutine 的上下文.

```go
func main() {
    ctx, cancel := context.WithCancel(context.Background())
    go func(ctx context.Context) {
		for {
			select {
			case <-ctx.Done():
				fmt.Println("Context canceled")
				return
			}
		}
	}(ctx)
  // 给所有监听 ctx.Done() 的通道发信号
	cancel()
}
```

函数 `context.Background` 返回一个空的 Context, 这个空的 Context 一般用于整个 Context 树的根节点.
Context 的衍生函数有:
* `context.WithCancel`: 返回一个可撤销的子 Context.

    `context.WithCancel` 函数返回两个值, 第一个是可被撤销的 Context, 第二个则是用于触发撤销信号的函数. 在撤销函数被调用后, 对应的 Context 值会先关闭它内部的接收通道, 也就是 `context.Done` 方法会返回的那个通道; 然后, 它会向它的所有子节点传达撤销信号; 最后, 这个 Context 值会断开它与其父值之间的关联.
    
* `context.WithDeadline`: 返回一个在指定时间后自动撤销的子 Context.
* `context.WithTimeout`: 返回一个可指定时长后自动撤销的子 Context.
* `context.WithValue`: 返回一个绑定了键值对(Key-Value)数据的子 Context. 此处需要自己保证 Value 的线程安全.

    此处的键值对是可被继承的. 在读取 Context 中存储的值时, 会沿着父子关系依次向上读取.
    
Context 的使用原则:
* 不要把 Context 放在结构体中, 要以参数的方式传递.
* 以 Context 作为参数的函数, 应该把 Context 作为首个参数.
* 给一个函数传递 Context 的时候, 不要传递 nil, 如果不知道传递什么, 就使用 `context.TODO`.
* Context 的 Value 相关方法应该只传递必须的数据.
* Context 是线程安全的, 可以放心的 goroutine 之间传递.

## Refs
* [来，控制一下 goroutine 的并发数量](https://github.com/EDDYCJY/blog/blob/master/talk/control-goroutine.md)


