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
[google/pprof](https://github.com/google/pprof) 是用于分析和可视化显示 profiling 数据的工具. 包括:
* CPU Profiling: CPU 分析. 按照一定的频率采集所监听的应用程序 CPU(令寄存器) 的使用情况, 可确定应用程序在主动消耗 CPU 周期时花费时间的位置.
* Memory Profiling: 内存分析. 在应用程序进行堆分配时记录堆栈跟踪, 用于监视当前和历史内存的使用情况, 以及检查内存泄漏.
* Block Profiling: 阻塞分析. 记录 Goroutine 阻塞等待同步(包括定时器通道)的位置.
* 

### Tools
* [divan/expvarmon](https://github.com/divan/expvarmon): TermUI based monitor for Go apps using expvars (/debug/vars). Quickest way to monitor your Go app(s).

### Refs
* [Go服务监控](https://www.cnblogs.com/52fhy/p/11828448.html)
