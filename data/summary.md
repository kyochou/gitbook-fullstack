# Data

## Processing
## Stream Processing
一般流式计算会与批量计算相比较. 在流式计算模型中, 输入是持续的, 可以认为在时间上是无界的, 也就意味着, 永远拿不到全量数据去做计算. 同时, 计算结果是持续输出的, 也即计算结果在时间上也是无界的. 流式计算一般对实时性要求较高, 同时一般是先定义目标计算, 然后数据到来之后将计算逻辑应用于数据. 同时为了提高计算效率, 往往尽可能采用增量计算代替全量计算.
![stream-processing](https://files-kyo.oss-cn-hongkong.aliyuncs.com/Fu3Ii_jVEq-gGg4lZJseygsB7dpr.png)


