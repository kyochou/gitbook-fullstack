# 压力测试

## Server
### Linux
* 关闭 SELinux: 使用命令 `sestatus` 查看; 使用命令 `setenforce 0` 临时关闭; 通过修改 `/etc/selinux/config` 文件(设置 `SELINUX=disabled`)永久关闭.
* 全局最大打开文件数. 使用命令 `cat /proc/sys/fs/file-nr | perl -F'\s+' -lane 'print $F[2]'` 查看.
    
    ```ini
    # vi /etc/sysctl.conf
    # 进程最大可打开文件数
    fs.nr_open = 1024000
    # 全局最大可打开文件数
    fs.file-max = 1024000
    # 修改后使用 sysctl -p /etc/sysctl.conf 重启服务
    ```
    
* 进程最大打开文件数. 使用命令 `ulimit -n` 查看; 使用命令 `ulimit -n 1024000` 临时修改; 或修改配置文件永远生效.

    ```ini
    # vi /etc/security/limits.conf
    * hard nofile 1024000
    * soft nofile 1024000
    
    ```
    
* 如果使用了 supervisor, 需要设置 supervisord 的 `minfs` 属性:

    ```ini
    # vi /etc/supervisor/supervisord.conf
    [supervisord]
    minfds=1024000
    ```    
    
* 查看运行中的进程限制: `cat /proc/<pid>/limits`.

### Refs
* [Increase the number of open files for jobs managed by supervisord](https://ma.ttias.be/increase-the-number-of-open-files-for-jobs-managed-by-supervisord/)
    
### Tools
* [ideawu/c1000k](https://github.com/ideawu/c1000k): A tool to test if you OS supports 1 million connections(c1000k).