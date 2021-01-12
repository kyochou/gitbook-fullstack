# Redis

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