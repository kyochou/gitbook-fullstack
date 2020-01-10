# sync.Pool
当多个 goroutine 都需要创建同一个对象的时候, 如果 goroutine 过多, 可能导致对象的创建数目剧增. 而对象又是占用内存的, 进而导致的就是内存回收的 GC 压力徒增. 造成 "并发大 -> 占用内存大 -> GC 缓慢 -> 处理并发能力降低 -> 并发更大" 这样的恶性循环. 这个时候, 我们非常迫切需要有一个对象池, 每个 goroutine 不再自己单独创建对象, 而是从对象池中获取出一个对象(如果池中已经有的话). 这就是 sync.Pool 出现的目的了.
`sync.Pool` 有两个公开方法. 一个是 Get, 另一个是 Put. 前者的功能是从池中获取一个 `interface{}` 类型的值, 而后者的作用则是把一个 `interface{}` 类型的值放置于池中.
`sync.Pool` 的定位不是做类似连接池的东西, 而是对对象的缓存. 它的用途仅仅是增加对象重用的几率, 减少 GC 的负担.

```go
const SIZE = 1024
var pool = sync.Pool{
    New: func() interface{} {
        return make([]int, SIZE)
    },
}

func main() {
    count := 10000000
    a := time.Now().Unix()
    for i := 0; i < count; i++{
        obj := make([]int, SIZE)
        obj = append(obj, 1)
    }
    b := time.Now().Unix()
    for i := 0; i < count; i++{
        obj := pool.Get().([]int)
        obj = append(obj, 1)
        pool.Put(obj)
    }
    c := time.Now().Unix()
    fmt.Println("without pool ", b - a, "s") // 16s
    fmt.Println("with    pool ", c - b, "s") // 1s
}
```

Pool 中存储的每一个值都应该是独立的, 平等的和可重用的. 我们应该既不用关心从池中拿到的是哪一个值, 也不用在意这个值是否已经被使用过.

