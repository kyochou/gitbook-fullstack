# CentOS

## YUM
### SCL
[Software collections(SCLs)](https://www.softwarecollections.org/) 是一个 Linux 软件多版本共存的解决方案, 适用于 RHEL/CentOS/Fedora. 
SCL 不会修改已安装软件, 也不会与其产生冲突.   
SCLs 相关的软件会安装在 `/opt/rh` 目录下.  
`scl` 命令用于激活 SCL 软件或在该软件环境下执行其他操作.   


```bash
# 添加源及工具
yum install scl-utils
# centos-release-scl 是 SCLs 的软件包
# centos-release-scl-rh 是 RedHat 的软件包
yum install centos-release-scl centos-release-scl-rh

# 安装 nginx
yum install -y rh-nginx116

# 注意, 基于 SCL 的软件在安装后默认并不会添加到环境变量中. 可通过使用 scl enable 命令进入其 Shell 环境
scl enable rh-nginx116 bash
# 或者将其添加到环境变量和系统服务中
source /opt/rh/rh-nginx116/enable
```


### FAQs
* 设置 yum 代理:
    添加代理设置如 `proxy=http://192.168.198.120:19999` 到 `/etc/yum.conf` 文件中.
    
* 查找 Yum 安装软件信息: `rpm -ql <name>`.  

## Network
### ip
CentOS 的网络 IP 配置文件在 `/etc/sysconfig/network-scripts/ifcfg-<网卡名>` 文件中.

自动获取:
```ini
TYPE="Ethernet"
BOOTPROTO="dhcp"
DEFROUTE="yes"
```

手动配置 IP:
```ini
TYPE="Ethernet"
BOOTPROTO="static"
IPADDR="192.168.199.82"
NETMASK="255.255.254.0"
GATEWAY="192.168.198.1"
DNS1="202.141.162.123"
DNS2="223.5.5.5"
DEFROUTE="yes"
```

### Refs
* [CentOS 上最佳的第三方仓库](https://linux.cn/article-8509-1.html)

## Resources
* [server-world](https://www.server-world.info/en/): 常用服务的最新版本安装命令.