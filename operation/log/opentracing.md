# OpenTracing

## 术语
* Trace
    一个 trace 代表一个事务或者流程在(分布式)系统中的执行过程.
    
* Span
    表示 trace 中的一个阶段. 记录 Trace 在执行过程中的信息. 多个 Span 之间可以是同级或父子关系.

## Jaeger

### Refs
* [jaeger-client-go/propagation_test.go](https://github.com/jaegertracing/jaeger-client-go/blob/master/propagation_test.go)
* [Uber 正式开源其分布式跟踪系统 Jaeger](https://www.infoq.cn/article/2017/11/Uber-open-spurce-Jaeger/)
* [研究调用链跟踪技术之jaeger](https://jeremyxu2010.github.io/2018/07/%E7%A0%94%E7%A9%B6%E8%B0%83%E7%94%A8%E9%93%BE%E8%B7%9F%E8%B8%AA%E6%8A%80%E6%9C%AF%E4%B9%8Bjaeger/)
* [微服务链路追踪之Jaeger](https://juejin.im/post/5de4a13fe51d454c95390218)

## Refs
* [night-reading-go/content/reading/29-2019-01-23-opentracing-jaeger-in-go.md](https://github.com/developer-learning/night-reading-go/blob/6453712b862700284c5989083016e0900e2a0d6b/content/reading/29-2019-01-23-opentracing-jaeger-in-go.md)
* [opentracing文档中文版](https://wu-sheng.gitbooks.io/opentracing-io/content/)
* [OpenTracing语义规范](https://segmentfault.com/a/1190000008895129)