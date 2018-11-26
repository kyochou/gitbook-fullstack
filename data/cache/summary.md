# Cache
## Redis
### CLI

```shell
$ # 删除所有的 key
$ redis-cli FLUSHALL
```

### FAQs
* 如何实现命名空间(库, 表, 字段)功能

    redis 不支持命名空间, 可以在设置 key 时使用以特殊符号(如 `:`)分隔的形式模拟. 如 `user:username`.