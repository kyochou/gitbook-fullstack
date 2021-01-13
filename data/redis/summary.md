# Redis

Redis 是基于 key/value 存储的单线程内存数据库.   

## Install
### 编译安装

```shell
# 自 [redis.io](https://redis.io/download) 下载源码
# 解压后 cd 到源码目录. 如 redis-6.0.9
cd redis-6.0.9
# 安装依赖, 并将 scl 的 gcc 添加到 path 中
yum install centos-release-scl scl-utils-build -y
sudo yum install -y devtoolset-9 tcl
source /opt/rh/devtoolset-9/enable
echo "source /opt/rh/devtoolset-9/enable" >> ~/.zshrc.local

# 给源码中的脚本添加执行权限
find . | grep -i '\.sh$' | xargs chmod +x
# 编译依赖
cd deps/jemalloc
chmod +x ./configure
./configure
cd ..
# 将 deps 目录中的文件夹都做为参数编译
make hiredis jemalloc linenoise lua
cd ..
make
# 查看编译结果
./src/redis-server --version
# 运行测试
chmod +x ./runtest
make test
# 安装
sudo make install PREFIX=/usr/local/redis6

```

## Run
### 优化
#### 系统优化
```shell
# run as root
echo madvise > /sys/kernel/mm/transparent_hugepage/enabled
sysctl vm.overcommit_memory=1
```

### 压测
```shell
# threads 参数的值最好与 Redis 配置中多线程的个数一致(如果开启)
# 基本上来说开启多线程会提高一倍的 qps
redis-benchmark -q --threads 4
```
#### Refs
* [Redis 6.0 多线程性能测试结果及分析](https://www.cnblogs.com/wy123/p/14180499.html)
* [How fast is Redis?](https://redis.io/topics/benchmarks)


## Tools
* [laixintao/iredis](https://github.com/laixintao/iredis): A Cli for Redis with AutoCompletion and Syntax Highlighting.   
