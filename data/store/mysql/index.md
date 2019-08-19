# 索引 

## Generated Column
在 MySQL5.7 中, 引入了两种 Generated Column, 即 Virtual Generated Column 和 Stored Generated Column. SGC 会将列数据持久化到磁盘上, 而 VGC 列的数据是是每次实时计算所得.
虚拟列一般用于在其上建立索引以提高查询速度. 如优化 `like` 查询, `json` 列的查询等.