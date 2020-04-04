# Ops

## Practice
我们在发布一个 Go 应用时, 默认都会启用两个 http handler: 一个是 pprof, 方便线上动态追踪问题; 另一个是 prometheus 的 metrics, 这样就可以通过 grafana 准实时的监控当前 runtime 信息, 及时预警.

## FAQs
* 运行 `go build` 命令时, 使用 `ldflags` 选项可以修改代码中变量的值. 参考: [Go 编程: 写一个通用的项目版本信息包](https://www.gitdig.com/go-build-version/).

## Refs
* [从nginx热更新聊一聊Golang中的热更新（下）](https://zhuanlan.zhihu.com/p/59196185)


## Build
### Tools
* [gobuffalo/packr](https://github.com/gobuffalo/packr): The simple and easy way to embed static files into Go binaries.


## Performance
### Memory
#### Refs
* [如何分析golang程序的内存使用情况](https://pengrl.com/p/24169/)

### Tools
* [divan/expvarmon](https://github.com/divan/expvarmon): TermUI based monitor for Go apps using expvars (/debug/vars). Quickest way to monitor your Go app(s).

#### [PProf](pprof.md)

#### GODEBUG
环境变量 GODEBUG 可以控制运行时的程序调试变量. 多个参数以逗号分隔, 格式为 `name=val`.

* [用 GODEBUG 看调度跟踪](https://github.com/eddycjy/blog/blob/master/tools/godebug-sched.md)
* [用 GODEBUG 看 GC](https://github.com/eddycjy/blog/blob/master/tools/godebug-gc.md)

#### Go trace
* [深入浅出 Go trace](https://www.itcodemonkey.com/article/5419.html)
* [Go 大杀器之跟踪剖析 trace](https://github.com/eddycjy/blog/blob/master/tools/go-tool-trace.md)

### Refs
* [Go服务监控](https://www.cnblogs.com/52fhy/p/11828448.html)
* [Diagnostics](https://cyningsun.github.io/07-21-2019/go-diagnostics-cn.html)
* [Go性能分析工具工具和手段](https://colobu.com/2019/05/22/profilinggo/)
* [滴滴Go实战：高频服务接口超时排查&性能调优](https://mp.weixin.qq.com/s/8l2Qf2vozhCcb9AvvJSBYA)