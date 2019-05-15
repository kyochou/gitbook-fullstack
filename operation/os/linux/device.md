# Device

## Disk
常用挂载目录:
* /home: 适用于用户多, 用户私有数据多的情况. 比如 ftp 服务器.
* /usr/local: 自编译安装的程序一般在这个目录. 生产环境的话一般推荐使用自编译安装软件的方式, 便于升级和备份.
* /var/{log,lib}: 使用安装工具(apt, yum 等)安装的软件会将日志(/var/log)和数据(/var/lib)放在此目录下. 

为保持一致性, 不管使用何种方式安装的软件, 统一将日志放在 `/var/log` 目录, 数据放在 `/var/lib` 目录. 即**将 /var  目录单独挂载**, 以便于扩展.


## CPU

```shell
# 型号
cat /proc/cpuinfo | grep 'model name' | sort | uniq
# 物理 cpu 数量
cat /proc/cpuinfo | grep 'physical id' | sort | uniq | wc -l
# 逻辑 CPU 核数 =物理cpu数量 x cpu cores 这个规格值 x 2(如果支持并开启超线程)。


```

### Refs
* [在Linux中查询CPU的核数](https://colobu.com/2019/02/22/how-to-find-cpu-cores-in-linux/)