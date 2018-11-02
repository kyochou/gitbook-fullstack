# FTP

## 选择
套用网上的总结:

* 假如你是需要一个支持很多外部模块和灵活配置的 FTP 服务器, 那你可以选择 ProFTPd.
* 假如你是需要一个很容易配置而且很安全的 FTP 服务器, 向你推荐 PureFTPd.
* 假如你需要的是超高性能且高安全性的 FTP 服务器, 那就是 VSFTPd.

当然做为一般使用三种 FTP 都可以, 考虑的中文的支持, 选择 PureFTPd(可以设置客户端默认编码) 可能会好一些.

## 安装
我这里使用的 Docker 版本是 1.6.2, 基础镜像是 Ubuntu14.04, 默认情况下 PureFTPd 无法直接通过 apt-get 进行安装(因内核没有开启 capabilities), 需要下载源码自行编译:

```
RUN mkdir pure-ftpd-source && \
  cd pure-ftpd-source && \
  apt-get source pure-ftpd && \
  cd pure-ftpd* && \
  sed -i '/^optflags=/ s/$/ --with-virtualchroot --without-capabilities/g' ./debian/rules && \
  apt-get build-dep -y pure-ftpd && \
  dpkg-buildpackage -uc -b && \
  dpkg -i ../pure-ftpd-common*.deb && \
  aptitude install -y openbsd-inetd && \
  dpkg -i ../pure-ftpd_*.deb
```

## 在 Docker 中的坑
0. 端口
  先来回顾下 FTP 的工作模式:
  * Standard 模式: 即 PORT, 主动模式. 此模式下客户端会发送一条命令告诉服务器客户端在本地打开了一个端口等待数据连接, 当服务器端收到 Port 命令后, 就会向客户端打开的端口进行连接, 从而进行数据传输.
  * Passive 模式: 即 PASV, 被动模式. 此模式下, 在建立连接后, 服务端会告诉客户端服务器打开了一个本地端口, 让客户端进行连接, 进而进行数据传输.

  如果使用 PSAV, 因为 Docker 需要与宿主机进行端口映射, 就需要为 FTP 指定所要使用的端口范围(需要在 PureFtpd 与 Docker 中同时指定):
  ```PureFTPd
   -p 30000:30021
   ```
  ```Docker
  EXPOSE 30000-30021
  docker run ... -p 30000-30021:30000-30021 ...
  ```
0. IP
  因 Docker 工作在宿主机上, 因此需要为 PureFTPd 指定工作在 PASV 模式下的 IP 为宿主机的 IP(通过 -P 参数).

0. 日志
  Ubuntu 的基础镜像默认是没有安装日志服务的, 需要 apt-get install rsyslog 进行安装. PureFTPd 中使用 -d 参数开启 verbose 模式.

0. 权限
  在通过 Volume 将 Docker 中的 FTP 目录与本地目录进行映射时, 出现了一些比较麻烦的问题:
  * 用户权限: 因为 FTP 是运行在 Docker 中, 所以上传文件的所有者也是 Docker 中的用户, 直接映射到本地就会出现用户 ID 错乱的情况(宿主机用户与 Docker 用户), 目前一个拆中的解决办法是在宿主机与 Docker 中建立同 ID 的用户, FTP 中使用虚拟用户以共享同一用户 ID.
  * 链接和挂载: 将目录以 rw 模式挂载到 Docker 中时, 首先软链接是不支持的, 如果要链接目录, 可以通过 bind 的方式实现(mount --bind 或 bindfs), 使用 bindfs 可以同时指定 bind 目录所属的用户和组. 但 Docker 不支持动态 bind, 执行 bind 操作后必须要重启 container 才能生效.



## Refs
* [Pure-FTPd](http://www.pureftpd.org/project/pure-ftpd)
* [stilliard/pure-ftpd](https://registry.hub.docker.com/u/stilliard/pure-ftpd/)
* [Setting up PureFTPD on a virtual server](http://www.dikant.de/2009/01/22/setting-up-pureftpd-on-a-virtual-server/)
* [Ubuntu Server 10.04下pure-ftpd工作方式和原理](http://os.51cto.com/art/201103/246820.htm)
* [What is the (best) way to manage permissions for docker shared volumes](http://stackoverflow.com/questions/23544282/what-is-the-best-way-to-manage-permissions-for-docker-shared-volumes)
* [Volumes files have root owner when running docker with non-root user. #3124](https://github.com/docker/docker/issues/3124)
* [Managing data in containers](https://docs.docker.com/userguide/dockervolumes/)
* [BindFS in cointainers](https://github.com/dalguete/docker/tree/master/bindfs)




