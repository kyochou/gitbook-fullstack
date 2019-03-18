# Device
## 路由器
* 将路由做为交换机使用

    1. 关闭路由器的 DHCP 功能.
    2. 将进口线和出口线(可有多条)均插在 LAN 口中即可.

### 极路由
* 刷机

    1. 安装插件 `vsftpd` 和 `定时重播`. 注意, `定时重播` 插件已下架, 任意打开一个可用插件然后将其 `sid` 改为 `118284854` 即可安装.
    2. 使用上面两个插件在路由器内执行包含以下代码的脚本文件以打开 `ssh` 服务:
    
        ```bash
        #!/bin/bash
        
        sed -i 's/1022/22/g' /etc/config/dropbear 
        /etc/init.d/dropbear enable
        /etc/init.d/dropbear start
        ```
        
    1. 安装相应版本的 [breed](https://breed.hackpascal.net/).
    2. 备份, 刷机.
    3. 参考:
        1. [极路由 1S（HC5661A）开启 SSH 功能](https://blog.csdn.net/m0_37651140/article/details/88085076)
        2. [极路由 B70 免开发者开启 ROOT 权限](https://www.quarkbook.com/?p=205)
        3. [分享下极路由 4 增强版刷 BREED 和 2019-1 月最新潘多拉固件](https://www.right.com.cn/forum/forum.php?mod=viewthread&tid=338628)

        