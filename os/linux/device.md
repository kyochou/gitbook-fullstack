# Device

使用 `lscpu` 命令可查看机器 cpu/vcpu 相关信息(`CPU(s)` 表示核数), 使用 `lsmem` 命令可查看内存相关信息, 使用 `lsblk` 可查看块设备(block)相关的信息.   

## Disk
常用挂载目录:
* /home: 适用于用户多, 用户私有数据多的情况. 比如 ftp 服务器.
* /usr/local: 自编译安装的程序一般在这个目录. 生产环境的话一般推荐使用自编译安装软件的方式, 便于升级和备份.
* /var/{log,lib}: 使用安装工具(apt, yum 等)安装的软件会将日志(/var/log)和数据(/var/lib)放在此目录下. 

为保持一致性, 不管使用何种方式安装的软件, 统一将日志放在 `/var/log` 目录, 数据放在 `/var/lib` 目录. 即**将 /var  目录单独挂载**, 以便于扩展.


块设备 IO 性能测试:   
```shell
sudo name="fio-`date "+%s"`" fio --name="$name" --filename="./$name" --rw=randread --refill_buffers --bs=4k --size=1G -runtime=10 -direct=1 -iodepth=128 -ioengine=libaio && rm -f "./$name"
```



## CPU

```shell
# 型号
cat /proc/cpuinfo | grep 'model name' | sort | uniq
# 物理 cpu 颗数
cat /proc/cpuinfo | grep 'physical id' | sort | uniq | wc -l
# 单颗物理 cpu 内核数
cat /proc/cpuinfo |grep "cores"|uniq|awk '{print $4}'
# cpu 逻辑线程数 = 物理 cpu 颗数 * 单颗物理 cpu 内核数 * 2(如果支持并开启超线程)
cat /proc/cpuinfo |grep "processor"|wc -l
```

### Refs
* [在Linux中查询CPU的核数](https://colobu.com/2019/02/22/how-to-find-cpu-cores-in-linux/)