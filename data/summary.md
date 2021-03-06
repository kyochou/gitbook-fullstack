# Data

## Processing
## Stream Processing
一般流式计算会与批量计算相比较. 在流式计算模型中, 输入是持续的, 可以认为在时间上是无界的, 也就意味着, 永远拿不到全量数据去做计算. 同时, 计算结果是持续输出的, 也即计算结果在时间上也是无界的. 流式计算一般对实时性要求较高, 同时一般是先定义目标计算, 然后数据到来之后将计算逻辑应用于数据. 同时为了提高计算效率, 往往尽可能采用增量计算代替全量计算.
![stream-processing](https://files-kyo.oss-cn-hongkong.aliyuncs.com/Fu3Ii_jVEq-gGg4lZJseygsB7dpr.png)

批量处理模型中, 一般先有全量数据集, 然后定义计算逻辑, 并将计算应用于全量数据. 特点是全量计算, 并且计算结果一次性全量输出.
![batch-processing](https://files-kyo.oss-cn-hongkong.aliyuncs.com/FjBbm9hTJL_ZHWmbS_GPSj302Rn0.png)

## Refs
* [Kafka 设计解析（七）- Kafka Stream](http://www.jasongj.com/kafka/kafka_stream/)
* [什么是流式计算 另一个世界系列](https://cloud.tencent.com/developer/article/1017092)