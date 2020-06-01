# Redis

Redis 是基于 key/value 存储的单线程内存数据库.   

## Usage
### List
#### Stack
* lpush + lpop   
* rpush + rpop   

#### Queue
* lpush + rpop    
* rpush + lpop    

#### Capped List(定长列表)   
* lpush/rpush + ltrim    

#### Message Queue
* lpush + brpop   
* rpush + blpop    

### Set(无序集合)
* 交集: sinter set1 set2 ...   
* 差集: sdiff set1 set2 ...   
* 并集: sunion set1 set2 ...    

### ZSet(有序集合)   


### HyperLogLog
基数计数(cardinality counting)通常用来统计一个集合中不重复的元素个数. 例如统计某个网站的 UV, 或者用户搜索网站的关键词数量. 数据分析, 网络监控及数据库优化等领域都会涉及到基数计数的需求.    
HyperLogLog 是一种实现了基数计数的算法. 可以利用极小的内存空间完成独立总数的统计, 但其会有一定的误差率(0.81%). 其适用于以下场景:
* 只为了计算独立总数, 不需要获取单条数据.     
* 统计对象数据量大, 可以容忍一定误差率, 毕竟 HyperLogLog 在内存的占用量上有很大的优势.   
* 可以和 Bitmaps 配合使用, 小数据量计数使用 Bitmaps, 大数据量使用 HyperLogLog 计数.  

### 事务
Redis 通过 `MULTI`, `DISCARD`, `EXEC`, `WATCH` 四个命令来实现事务功能.  

#### Refs
* [Redis 设计与实现 事务](https://redisbook.readthedocs.io/en/latest/feature/transaction.html)

## FAQs
* 使用正则删除 key:  
    
    ```shell
    redis-cli --scan --pattern users:* | xargs redis-cli del 
    # redis >= 4.0
    redis-cli --scan --pattern users:* | xargs redis-cli unlink
    ```


## Refs
* [基于Redis实现消息队列典型方案](http://www.hellokang.net/redis/message-queue-by-redis.html)