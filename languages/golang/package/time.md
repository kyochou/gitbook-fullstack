# Time
## Location
time 包提供了 Location 的两个实例: Local 和 UTC. Local 代表当前系统本地时区; UTC 代表通用协调时间, 也就是零时区.
通常, 我们使用 `time.Local` 即可, 偶尔需要使用 `time.UTC`.
函数 `LoadLocation(name string) (*Location, error)` 可以根据名称获取特定时区的实例.

## Time
`Time` 代表一个纳秒精度的时间点.
每一个 `Time` 都具有一个地点信息(`time.Location`).
表示时间的变量或字段, 应为 `time.Time` 类型, 而不是 `*time.Time`. 一个 `Time` 类型值可以被多个协程同时使用.
时间点可以使用 `Before, After, Equal` 方法进行比较. `Add, Sub` 方法可对时间点进行增减.
`Time.IsZero()` 函数用于判断 Time 表示的时间点是否为零值.

### Unix timestamp
* `time.Unix(sec, nsec int64)`: 通过 Unix 时间戳生成 `time.Time` 实例;
* `time.Time.Unix()`: 得到 Unix 时间戳.
* `time.Time.UnixNano()`: 

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
