# RaspberryPi
## 4B
### Hardware
[接口图示](https://img.alicdn.com/imgextra/i4/2206530532867/O1CN01ZL9ZPk1X385HNnOpj_!!2206530532867.jpg)

### System

```shell

## 设置默认启动界面
sudo raspi-config
# `System Options` -> `Boot / Auto Login`

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