# RaspberryPi
## Hardware
[4B 接口图示](https://img.alicdn.com/imgextra/i4/2206530532867/O1CN01ZL9ZPk1X385HNnOpj_!!2206530532867.jpg)

## System

### Config
```shell
# 使用 sudo service --status-all 命令查看服务的状态
sudo raspi-config
## 设置默认启动界面
# `System Options` -> `Boot / Auto Login`
## 开启 ssh
# `System Options` -> `Interface Options -> SSH`
## 设置 wifi
# `System Options` -> `Wireless LAN`
## 设置时区, 语言等
# `Localisation Options` -> `Wireless LAN`
```

### Network 
```ini
## 使用固定 IP 地址
## RaspberryPi 的 bullseye 64bit 版本是使用 dhcpcd 服务来管理的网络, 并没有使用 networking
## sudo vi /etc/dhcpcd.conf
interface wlan0
static ip_address=192.168.1.88/24
# static ip6_address=fd51:42f8:caae:d92e::ff/64
static routers=192.168.1.1
static domain_name_servers=1.1.1.1 223.5.5.5 180.76.76.76 8.8.8.8

interface eth0
static ip_address=192.168.1.66/24
# static ip6_address=fd51:42f8:caae:d92e::ff/64
static routers=192.168.1.1
static domain_name_servers=1.1.1.1 223.5.5.5 180.76.76.76 8.8.8.8

## 配置好后重启下 Pi: sudo reboot
```

```ini
# vi /etc/network/interfaces 
# 备用 IP 设置.
auto eth0
iface eth0 inet static
address 192.168.1.66
gateway 192.168.1.1
netmask 255.255.255.0
dns-nameservers 1.1.1.1 223.5.5.5 180.76.76.76 8.8.8.8

auto wlan0
iface wlan0 inet static
address 192.168.1.88
gateway 192.168.1.1
netmask 255.255.255.0
dns-nameservers 1.1.1.1 223.5.5.5 180.76.76.76 8.8.8.8
```
### Sources

```ini
# 修改源 sudo vi /etc/apt/sources.list

# https://mirrors.ustc.edu.cn/repogen/
deb https://mirrors.ustc.edu.cn/debian/ bullseye main contrib non-free
deb-src https://mirrors.ustc.edu.cn/debian/ bullseye main contrib non-free
deb https://mirrors.ustc.edu.cn/debian/ bullseye-updates main contrib non-free
deb-src https://mirrors.ustc.edu.cn/debian/ bullseye-updates main contrib non-free
deb https://mirrors.ustc.edu.cn/debian/ bullseye-backports main contrib non-free
deb-src https://mirrors.ustc.edu.cn/debian/ bullseye-backports main contrib non-free
deb https://mirrors.ustc.edu.cn/debian-security/ bullseye-security main contrib non-free
deb-src https://mirrors.ustc.edu.cn/debian-security/ bullseye-security main contrib non-free

```

```shell
sudo apt-get update && sudo apt-get upgrade -y
```

### Tools
```shell
sudo apt-get install postfix mailutils -y
sudo apt-get install vim -y && echo 'alias vi=vim' >> .bashrc
```

## Apps
### Samba
```shell
# smbnetfs 用于将服务广播到网络中
sudo apt-get install -y samba smbnetfs
# 添加用户
smbpasswd -a kyo 
```
```ini
# sudo vi /etc/samba/smb.conf

[global]
  workgroup = WORKGROUP
  # 兼容小米摄像头
  server min protocol = NT1
  
# 共享目录
[shared]
  comment = Shared on Pi
  # 注意要把这个目录权限改为 777, 以便匿名用户写入
  path = /mnt/data/shared
  public = yes
  writable = yes
  guest ok = yes
  create mode = 0666
  directory mode = 1777

# 小米电视的应用中找到 "高清播放器", 然后 "设备->添加设备" 即可
[videos]
  comment = Videos On Pi
  path = /mnt/raid1/media/video
  available = yes
  browseable = yes
  read only = yes
  public = yes

# 小米摄像头要求必须使用用户登录
[xiaomi-camera]
  comment = xiaomi camera
  path = /mnt/data/yuntai
  available = yes
  browseable = yes
  writable = yes
  public = no

```

```shell
sudo service smbd restart && sudo service nmbd restart
```

### TimeMachine 支持

```shell
sudo apt install netatalk avahi-daemon -y
```
```ini
# sudo vim /etc/netatalk/afp.conf
 [Time Machine Server Name]
  path = /path/to/back/timemachine
  time machine = yes
```

```shell
sudo service netatalk restart
sudo service avahi-daemon restart

# 然后在 Mac 上就可以找到这个配置的磁盘了
```

#### Refs
* [从此Mac上的文件再也不会丟了，我来教你使用树莓派做无线时间机器](https://zhuanlan.zhihu.com/p/335259509)


### polipo
```shell
# 由于 polipo 已停止了维护. debian bullseye 上已经没有这个软件的源了. 需要下载 deb 文件安装.
sudo dpkg -i /mnt/raid1/linux/debian/polipo_1.1.1-10_arm64.deb

```

```ini
# 添加配置到 /etc/polipo/config
socksParentProxy = 127.0.0.1:19911
socksProxyType = socks5

proxyAddress = 0.0.0.0
proxyPort = 19999
```

```shell
# 重启服务
sudo service polipo restart && sudo service polipo status
```

### Syncthing

* [Syncthing](https://syncthing.net/)
* [详细步骤记录如何用Syncthing实时同步树莓派上的文件](https://www.labno3.com/2021/03/31/synchronizing-files-on-your-raspberry-pi-with-syncthing/)


