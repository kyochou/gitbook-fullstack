# 测试
我们可以为 Go 程序编写三类测试, 即: 单元测试(test, 也称为功能测试), 基准测试(benchmark, 也称性能测试), 以及示例测试(example).
`go test` 是一个按照一定的约定和组织的测试代码的驱动程序. 在包目录内(源码相同目录下), 以 `_test.go` 为后缀名的文件(被测试的代码文件名加上后缀 "_test", 使用和被测试文件相同的包名)并不是 `go build` 构建包的一部分, 它们是 `go test` 测试的一部分.
测试用例有四种形式:
* `TestXxx(t *testing.T)`: 单元测试用例.
* `BenchmarkXxx(b *testing.B)`: 压力测试用例.
* `ExampleXxx()`: 测试控制台输出.
* `TestMain(m *testing.M)`: 测试 Main 函数.

## testing.T
默认情况下, 单元测试成功时, 它们打印的信息不会输出, 可以通过加 `-v` 选项输出这些信息. 但对于基准测试, 它们总会被输出.
`T.Parallel` 方法用于表示当前测试只会与其他带有 `Parallel` 方法的测试并行进行测试.

## 单元测试
单元测试的意思是: 对单一的功能模块进行边界清晰的测试, 并且不掺杂任何对外部环境的检测.
对于功能测试函数, 名称必须以 `Test` 为前缀, 并且参数列表中只应有一个 `*testing.T` 类型的参数声明.
如果想让某个测试函数在执行过程中立即失败并返回, 可调用 `t.FailNow` 方法.
`t.Error` 方法相当于 `t.Log` 方法和 `t.Fail` 方法的连续调用. `t.Errorf` 方法同理.
`功能测试函数的执行次数 = "-cpu 参数的值中正整数的个数" * 
-count 标记的值"`

## 基准测试
基准测试一般都是串行进行的.
对于性能测试函数, 名称必须以 Benchmark 为前缀, 并且唯一参数的类型必须为 `*testing.B` 类型.
性能测试函数的执行次数 = `-cpu` 参数的值中正整数的个数 * `-count` 参数的值 * 探索式执行中测试函数的实际执行次数.
使用 `-benchmem` 选项可以打印测试执行的内存占用情况.

```shell
$ go test -benchmem  -bench=.
BenchmarkMakeWithPool-8(1)         20000000(2)                65.9 ns/op(3)            78 B/op(4)          1 allocs/op(5)

```
输出结果分别为:
1. 执行测试的名称. 后面的数字(8)表示执行时所用的最大逻辑处理器(Processor)的数量.
2. 测试执行次数.
3. 测试的单次执行时间.
4. 测试的单次执行所占内存量.
5. 测试的单次执行所进行的内存分配次数.

## 示例测试
对于示例测试, 其名称必须以 Example 为前缀, 对函数参数没有要求.
Example 需要在最后用注释的方式确认控制台输出和预期的是否一致.

```go
func ExampleFunc() {
	fmt.Println(31.2)
	// Output:
	// 31.1
}
```    

## Tools
* [smartystreets/goconvey](https://github.com/smartystreets/goconvey): Go testing in the browser. Integrates with `go test`. Write behavioral tests in Go.

### metrics
* [rcrowley/go-metrics](https://github.com/rcrowley/go-metrics): 用来为某个事件做监控, 统计.
* [armon/go-metrics](https://github.com/armon/go-metrics): A Golang library for exporting performance and runtime metrics to external metrics systems (i.e. statsite, statsd).


#### Refs
* [golang metrics 的基本使用](https://zhuanlan.zhihu.com/p/30441529)
* [go 服务监控指标(metric)上报open-falcon](https://juejin.im/post/5ae1b1a7518825672f199435)
* [使用Metrics监控应用程序的性能](https://www.cnblogs.com/yangecnu/p/Using-Metrics-to-Profiling-WebService-Performance.html)


## Refs
* [polaris1119/The-Golang-Standard-Library-by-Example](https://github.com/polaris1119/The-Golang-Standard-Library-by-Example/blob/master/chapter09/09.0.md)
* [GO 中如何进行单元测试](http://blog.studygolang.com/2017/10/how-to-test-with-go/)