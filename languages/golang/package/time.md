# Time
## Timer
定时器. 用于在指定时间后执行代码.

```go
// 使当前 goroutine 休眠一秒
time.Sleep(time.Second * 1)

t1 := time.NewTimer(time.Second * 1)
<-t1.C // 一秒后此 timer 被触发, 其后的代码得以执行
fmt.Println(`t1 expired`)

t2 := time.NewTimer(time.Second)
go func() {
    <-t2.C // 计时器被中止后其后的代码不会被执行
    fmt.Println(`t2 expired`)
}()
// 使用 Stop 方法主动停止计时器
if t2.Stop() {
    fmt.Println("t2 stopped")
}
```

## Ticker
打点器. 用于在固定的间隔中执行代码.

```go
t := time.NewTicker(time.Second * 1)
go func() {
	for t := range t.C { // 使用 range 触发打点事件
		fmt.Println(`Tick at: `, t)
	}
}()

time.Sleep(time.Second * 3)
t.Stop() // 停止 ticker

```
