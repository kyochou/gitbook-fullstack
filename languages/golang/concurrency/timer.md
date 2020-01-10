# 定时器

Timer/Ticker 对象不再使用后, 应显式调用 `Stop` 方法释放资源:

```go
t := new time.NewTimer(5 * time.Second)
select {
case <- ch:
  // 做相关的业务
  // 此处应释放 t
  t.Stop()
case <- t:
  // 超时了，做超时处理
}
```