# ID
## 生成策略
1. 定义字符库. 如 "数字, 大写字母, 小写字母" 可以组成 62 位的字符库.
* 每次自字符集中随机取出一个, 循环多次即可生成指定位数的随机数. 字符库越大重复的几率越低.

## 常见策略

| Name | Binary Size | String Size | Features |
| :-: | :-: | :-: | :-: |
| [UUID](https://en.wikipedia.org/wiki/Universally_unique_identifier) | 16 bytes | 36 chars | configuration free, not sortable |
| [shortuuid](https://github.com/skorokithakis/shortuuid) | 16 bytes | 22 chars | configuration free, not sortable |
| [Snowflake](https://blog.twitter.com/engineering/en_us/a/2010/announcing-snowflake.html) | 8 bytes | up to 20 chars | needs machin/DC configuration, sortable, number only |
| [ObjectId](https://docs.mongodb.com/manual/reference/method/ObjectId/) | 12 bytes | 24 chars | configuration free, sortable |
| [xid](https://github.com/rs/xid) | 12 bytes | 20 chars | configuration free, sortable |

有纯数字需求的(如订单号, 数据库主键)可使用 Snowflake, 仅用来做唯一 ID 使用的可使用 ObjectId.


* [Meituan-Dianping/Leaf](https://github.com/Meituan-Dianping/Leaf): 美团开源的, 基于现有 ID 生成策略改进的分布式 ID 生成服务.
* [baidu/uid-generator](https://github.com/baidu/uid-generator): 百度基于 Snowflake 的 ID 生成器.
* [didi/tinyid](https://github.com/didi/tinyid): 滴滴 ID 生成器.

### Golang
* [rs/xid](https://github.com/rs/xid): 基于 MongoID(ObjectID) 算法, 使用更多的元字符数生成更少位数的唯一 ID.
* [sony/sonyflake](https://github.com/sony/sonyflake): Snowflake 的 Golang 实现.

### Refs
* [Human Readable IDs](https://h13g.com/read?id=28)