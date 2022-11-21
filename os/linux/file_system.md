# FileSystem

## mount
一般情况下存储设备不分区也可以直接进行格式化然后挂载, 但在一些情况下(有可能是设备空间太大, 或使用的外接硬盘架有自动休眠功能等), 不格式化会导致系统在重启时无法加载到特定的存储设备(如 raspberrypi4b 的外接硬盘盒), 因此对存储设备的使用最好统一遵守 "分区 -> 格式化 -> 挂载" 这一步骤, 防止出现未知的问题.   

```shell
## 查看接入的块设备
lsblk -f
# 使用 blkid 命令可查看设备的详细信息

## 分区, fdisk 的最大分区容量是 2T. 对更大的分区使用 gdisk 来操作
sudo gdisk

## 格式化
sudo mkfs.ext4 /dev/sda

## 挂载
sudo mkdir -p /mnt/data
sudo mount -t ext4 /dev/sda /mnt/data
# 卸载
# sudo umount /mnt/data

## 开机自动挂载
# 使用 nofail 标志防止设备启动失败从而导致整个系统的无法启动
echo 'UUID=<uuid_in_lsblk> /mnt/data ext4 defaults,nofail 0 0' | sudo tee -a /etc/fstab
```

## raid

```shell
sudo apt-get install mdadm -y
# 加载 raid 模块
sudo reboot

# 查看目录设备
lsblk -f
# 对所有的目标设备进行分区
sudo gdisk
# 创建 raid1
sudo mdadm --create /dev/md0 --level=mirror --raid-devices=2 /dev/sdb /dev/sdc
# 查看 raid 状态
sudo mdadm --detail /dev/md0
# 保存 raid 信息
sudo mdadm --detail --scan --verbose | perl -pe's/\n/ /g' | sudo tee -a /etc/mdadm/mdadm.conf
# 检查配置文件
sudo mdadm -D --scan

# 格式化, 挂载
sudo mkfs.ext4 /dev/md0
sudo mkdir -p /mnt/raid1
sudo mount /dev/md0 /mnt/raid1
# 开机自动挂载
echo 'UUID=<uuid_in_lsblk> /mnt/raid1 ext4 defaults,nofail 0 0' | sudo tee -a /etc/fstab

```

```ini
## 配置邮箱预警
# 需要先配置好 ssmtp
# sudo vim /etc/mdadm/mdadm.conf

# instruct the monitoring daemon where to send mail alerts
MAILADDR develop.kyo@gmail.com
MAILFROM noreply_kyo@163.com

```

```shell
## 其他操作

# 测试能否收到邮件
sudo mdadm --monitor /dev/md0 --test

# 查看状态
sudo mdadm --detail /dev/md0
cat /proc/mdstat

# 开启 pending 状态的 raid 设备
mdadm --readwrite /dev/md1

# 关闭 raid
umount /dev/md0
mdadm --stop /dev/md0
# 清除 raid 配置信息
mdadm --zero-superblock /dev/sda1 /dev/sdb1

```
### Refs
* [Debian 组建软 RAID 1 磁盘阵列](https://www.dreamxu.com/debian-create-software-raid-one-arrays/)
* [Software RAID on Raspberry Pi](https://www.spatacoli.com/blog/2022/01/software-raid-on-rpi/)
* [How to clear up pending resync on RAID array](https://sleeplessbeastie.eu/2015/03/23/how-to-clear-up-pending-resync-on-raid-array/)
* [Removal of mdadm RAID Devices – How to do it quickly?](https://bobcares.com/blog/removal-of-mdadm-raid-devices/)
* [用Raspberry Pi搭建NAS服务](https://www.jianshu.com/p/f094f76f6ee3)

## fstab
因挂载的设备所属的设备名(e.g. /dev/sda, /dev/sdb)有可能发生变化, 因此在设置挂载时最好统一用 UUID 来标识设备.  
