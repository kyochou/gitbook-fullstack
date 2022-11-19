# RaspberryPi
## 4B
### Hardware
[接口图示](https://img.alicdn.com/imgextra/i4/2206530532867/O1CN01ZL9ZPk1X385HNnOpj_!!2206530532867.jpg)

### System

```shell
# 使用 sudo service --status-all 命令查看服务的状态
sudo raspi-config
## 设置默认启动界面
# `System Options` -> `Boot / Auto Login`
## 开启 ssh
# `System Options` -> `Interface Options -> SSH`

```

#### network 
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

#### Refs
* [树莓派格式化u盘（硬盘）ext4格式并挂载](https://www.jianshu.com/p/4a8d7ecddeec)

### Apps
### 视频播放
* 安装 samba 服务: `sudo apt-get install -y samba`
* 修改配置(`sudo vi /etc/samba/smb.conf`)并重启服务:  

    ```ini
    [videos]
       comment = Pi Videos
       path = /media/video
       browseable = yes
       read only = yes
       public = yes
    ```
    
* 小米电视的应用中找到 "高清播放器", 然后 "设备->添加设备" 即可.


#### Refs
* [从此Mac上的文件再也不会丟了，我来教你使用树莓派做无线时间机器](https://zhuanlan.zhihu.com/p/335259509)