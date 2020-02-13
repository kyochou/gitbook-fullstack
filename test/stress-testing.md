# 压力测试

## Server
### Linux
* 关闭 SELinux: 使用命令 `sestatus` 查看; 使用命令 `setenforce 0` 临时关闭; 通过修改 `/etc/selinux/config` 文件(设置 `SELINUX=disabled`)永久关闭.
* 全局最大打开文件数. 使用命令 `cat /proc/sys/fs/file-nr | perl -F'\s+' -lane 'print $F[2]'` 查看.
    
    ```ini
    ## vi /etc/sysctl.conf
    ## 修改后使用 sysctl -p /etc/sysctl.conf 重启服务
    
    # 进程最大可打开文件数
    fs.nr_open = 1024000
    # 全局最大可打开文件数
    fs.file-max = 1024000
    # iptables 会使用 nf_conntrack 模块跟踪连接
    # net.nf_conntrack_max = 1024000
    
    # 网络设置
    net.ipv4.ip_forward=1
    # 全连接队列最大长度
    #net.core.somaxconn = 2048
    # SYN_RCVD 状态连接的最大个数
    #net.ipv4.tcp_max_syn_backlog = 2048
    # 接收自网卡, 但未被内核协议栈处理的报文队列长度
    #net.core.netdev_max_backlog = 2048
    # 超出处理能力时, 对新来的 SYN 直接回包 RST, 丢弃连接
    net.ipv4.tcp_abort_on_overflow = 1
    
    ## time-wait 优化
    # 开启后, 作为客户端时新连接可以使用仍然处于 TIME-WAIT 状态的端口.
    net.ipv4.tcp_tw_reuse = 1
    # 开启后, 操作系统可以拒绝迟到的报文(与 `tcp_tw_reuse` 配合使用)
    net.ipv4.tcp_timestamps = 1
    # 开启后, 同时作为客户端和服务器都可以使用 TIME-WAIT 状态的端口. 不安全, 无法避免报文延迟, 重复等问题给新连接千万混乱.
    # net.ipv4.tcp_tw_recycle = 1
    # 设置 time_wait 状态连接的最大数量, 超出后直接关闭连接
    # net.ipv4.tcp_max_tw_buckets = 262144
    # sysctl -w /proc/sys/net/core/netdev_max_backlog=2048
    ```
    
* 进程最大打开文件数. 使用命令 `ulimit -n` 查看; 使用命令 `ulimit -n 1024000` 临时修改; 或修改配置文件永远生效.

    ```ini
    # vi /etc/security/limits.conf
    # open files, nofile 是每个进程可以打开的文件数限制.
    * hard nofile 1024000
    * soft nofile 1024000
    # max user processes, nproc 是每个用户可以创建的进程数限制.
    * soft nproc 1024000
    * hard nproc 1024000
    
    ```
    
* 修改 `max user processes`. 注意, `/etc/security/limits.d/` 里面的配置会覆盖 `/etc/security/limits.conf` 的配置.

    ```ini
    # vim /etc/security/limits.d/20-nproc.conf
    *          soft    nproc     1024000

    ```    
    
* 如果使用了 supervisor, 需要设置 supervisord 的 `minfs` 属性:

    ```ini
    # vi /etc/supervisor/supervisord.conf
    [supervisord]
    minfds=1024000
    ```    
    
* 查看运行中的进程限制: `cat /proc/<pid>/limits`.

#### FAQs
* 进程运行过程中被 kill 掉
    一个可能的原因是, 进程被 Linux 的 `OOM killer` 杀掉. 可以执行 `dmesg | less` 命令通过搜索进程 ID 查看详细原因.

### Refs
* [Increase the number of open files for jobs managed by supervisord](https://ma.ttias.be/increase-the-number-of-open-files-for-jobs-managed-by-supervisord/)
* [突破操作系统limit的限制](https://mp.weixin.qq.com/s/JFTUWBJmWeRp-IMYJIND5A)
    
### Tools
* [ideawu/c1000k](https://github.com/ideawu/c1000k): A tool to test if you OS supports 1 million connections(c1000k).