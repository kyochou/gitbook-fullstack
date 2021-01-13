# Config

```ini
# 关闭 rdb. 注释掉所有 save 选项, 只保存下面一条
save ""
# 开启 rdb-aof 混合模式
appendonly yes
aof-use-rdb-preamble yes
# 开启多线程
io-threads 4
io-threads-do-reads yes
# 慢日志
slowlog-log-slower-than 100000
slowlog-max-len 128
```

## Refs
* [Redis 4.0 新功能简介：RDB-AOF 混合持久化](https://blog.huangz.me/2017/redis-rdb-aof-mixed-persistence.html)