# Ops

## FAQs
* 运行 `go build` 命令时, 使用 `ldflags` 选项可以修改代码中变量的值. 参考: [Go 编程: 写一个通用的项目版本信息包](https://www.gitdig.com/go-build-version/).

## Refs
* [从nginx热更新聊一聊Golang中的热更新（下）](https://zhuanlan.zhihu.com/p/59196185)


## Build
### Tools
* [gobuffalo/packr](https://github.com/gobuffalo/packr): The simple and easy way to embed static files into Go binaries.


## Performance
### PProf
Profiling 一般译成画像. 在计算机性能调试领域里, Profiling 就是对应用的画像, 即应用使用 CPU 和内存等资源的情况.
[google/pprof](https://github.com/google/pprof) 是用于分析和可视化显示 profiling 数据的工具. 包括:
* CPU Profiling: CPU 分析. 按照一定的频率采集所监听的应用程序 CPU(令寄存器) 的使用情况, 可确定应用程序在主动消耗 CPU 周期时花费时间的位置.
* Memory(Heap) Profiling: 内存分析. 在应用程序进行堆分配时记录堆栈跟踪, 用于监视当前和历史内存的使用情况, 以及检查内存泄漏. 每分配 `runtime.MemProfileRate` 个字节(默认为 512)进行一次数据采样.
* Block Profiling: 阻塞分析. 记录 Goroutine 阻塞等待同步(包括定时器通道)的位置, 可用来分析和查找死锁等性能瓶颈. 使用前需要先调用 `rumtime.SetBlockProfileRate`.
* Goroutine Profiling: 报告 Goroutines 的使用情况, 有哪些 goroutine, 它们的调用关系是怎样的.
* Mutex Profiling: 互斥锁分析. 报告互斥锁的竞争情况. 使用前需要先调用 `runtime.SetMutexProfileFraction`.

做 Profiling 的第一步就是怎么获取应用程序的运行情况数据. Go 提供了 `runtime/pprof` 和 `net/http/pprof` 两个库.

#### net/http/pprof
它们都支持名为 debug 的 URL 参数. 默认为 0, 此时返回的采样数据是不可人为解读的函数地址列表; 为 1 时, 会将函数地址转换为函数名; 对 goroutine 来说, 还支持值 2, 此时将以 `unrecovered panic` 的格式打印堆栈, 可读性更高. 

#### go tool pprof

通过命令 `go tool pprof $HOST/debug/pprof/profile`, 可默认进行 30s 的 CPU Profiling，得到一个分析用的 profile 文件`


#### go test 
通常用 `net/http/pprof` 或 `runtime/pprof` 对应用进行整体分析, 找出热点后, 再用 `go test` 进行基准测试, 进一步确定热点加以优化并对比测试.

#### Refs
* [go pprof 性能分析](https://juejin.im/entry/5ac9cf3a518825556534c76e)

### Tools
* [divan/expvarmon](https://github.com/divan/expvarmon): TermUI based monitor for Go apps using expvars (/debug/vars). Quickest way to monitor your Go app(s).

### Refs
* [Go服务监控](https://www.cnblogs.com/52fhy/p/11828448.html)
