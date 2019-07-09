# ID
## 生成策略
1. 定义字符库. 如 "数字, 大写字母, 小写字母" 可以组成 62 位的字符库.
* 每次自字符集中随机取出一个, 循环多次即可生成指定位数的随机数. 字符库越大重复的几率越低.

## 常见策略

| 名称 |  | 备注 |
| Name|	Binary Size	String Size	Features
| :-: | :-: | :-: |
| 2019-07-09 | 163.7 | |

* UUID: 16 bytes, 36 chars, configuration free, not sortable.
* shortuuid: 16 bytes, 22 chars, configuration free, not sortable.
* Snowflake: 8 bytes, up to 20 chars, needs machin/DC configuration, needs central server, sortable.
* MongoID: 12 bytes	24 chars	configuration free, sortable
xid	12 bytes	20 chars	configuration free, sortable

### Golang
* [rs/xid](https://github.com/rs/xid): 基于 MongoID(ObjectID) 算法, 使用更多的元字符数生成更少位数的唯一 ID.
* [sony/sonyflake](https://github.com/sony/sonyflake): Snowflake 的 Golang 实现.

### Refs
* [Human Readable IDs](https://h13g.com/read?id=28)