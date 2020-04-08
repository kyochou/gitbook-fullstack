# Math

## math/rand
使用 `rand.New()` 方法创建的 `Rand` 实例并不是并发安全的. 但其自带的 `Rand` 实例是有并发保护的. 所以, 尽量使用其自带的 `Rand` 实例. 可通过设置种子来防止相同随机数的问题.
```go

func init() {
	rand.Seed(time.Now().UnixNano())
}

```

### Refs
* [math/rand: document that math.Rand is not safe for concurrent use #3611](https://github.com/golang/go/issues/3611)