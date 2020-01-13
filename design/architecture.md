# Architecture
## HTTP
基于 HTTP 应用架构的关键属性:
* 性能(Performance).
    * 网络性能.
        * 吞吐量(Throughput).
        * 交互开销(Overhead): 首次开销, 每次开销.
    * 用户感知到的性能(User-perceived Performance).
        * 延迟(Latency): 发起请求到接收到响应的时间.
        * 完成时间(Completion): 完成一个应用动作所花费的时间.
    * 网络效率(Network Efficiency): 包括 重用缓存, 减少交互次数, 减小数据传输距离, COD(Code on Demand, 按需代码) 等.
* 可伸缩性(Scalability): 支持部署可以互相交互的大量组件.
* 简单性(Simplicity): 易理解, 易实现, 易验证.
* 可见性(Visiable): 对两个组件间的交互进行监视或仲裁的能力. 如缓存, 分层设计等. 
* 可移植性(Portability): 在不同的环境下运行的能力.
* 可靠性(Reliability): 出现部分故障时, 对整体影响的程度.
* 可修改性(Modifiability): 对系统作出修改的难易程度.
    * 可进化性(Evolvability): 一个组件独立升级而不影响其他组件.
    * 可扩展性(Extensibility): 向系统添加功能, 而不会影响到系统的其他部分.
    * 可定制性(Customizability): 临时性, 定制性地更改某一要素来提供服务, 不会对常规客户产生影响.
    * 可配置性(Configurability): 应用部署后可通过修改配置提供新的功能.
    * 可重用性(Reusabbility): 组件可以不做修改的被其他应用使用.

我们并不能让所有属性都达到一个非常好的标准. 只能在这些属性中取得可接受的均衡.
