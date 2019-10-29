# 操作符

## ...
```go
// 用于不定参数的定义
func Foo(args ...interface{}) {
  for _, arg := range args {
    fmt.Println(arg)
  }
}
// 用于传递不定参数
args := []interface{}{1, false, "hello"}
Foo(args...)

// 用于定义动态的数组长度
arr := [...]int{1,2,4}
```

## Refs
* [Go 编程：那些隐晦的操作符](https://www.gitdig.com/go-operators/)