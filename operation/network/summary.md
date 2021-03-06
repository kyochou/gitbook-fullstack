# Network

SOCKS 是一种网络传输协议, 主要用于客户端与外网服务器之间通讯的中间传递. SOCKS 是 "SOCKetS" 的缩写. 当防火墙后的客户端要访问外部的服务器时, 就跟 SOCKS 代理服务器连接. 这个代理服务器控制客户端访问外网的资格, 允许的话, 就将客户端的请求发往外部的服务器.

## Tools
### Capture
* [40t/go-sniffer](https://github.com/40t/go-sniffer): Sniffing and parsing mysql,redis,http,mongodb etc protocol. 

    
### Speed

* 使用 [sivel/speedtest-cli](https://github.com/sivel/speedtest-cli) 测试网络的上传和下载速度, 使用 [reorx/httpstat](https://github.com/reorx/httpstat) 测试网站的打开速度.
* [alexkirsz/dispatch-proxy](https://github.com/alexkirsz/dispatch-proxy): Combine internet connections, increase your download speed. 可同时使用多个网卡.

### DNS
* 将域名解析为指定的 IP(可使用内网地址): [xip.io](http://xip.io/), [nip.io](http://nip.io/).

## CDN
### Tools
* [CNDPerf](https://www.cdnperf.com/): 在线测试 CDN 性能的网站.

## CLI
### nc(netcat)
#### 安装

```shell
$ yum install nmap-nc -y
$ brew install netcat
```

#### 常用操作
* 使用 `-v` 选项可以显示命令细节(可用来代替 `telnet` 做测试端口): `nc -v <IP> <PORT>`.
* 使用 `-c` 或 `-e` 选项可在客户端连接后执行命令: `nc -l <PORT> -e /bin/bash`; 或实现端口转发(如 80 转发到 8080): `nc -l 80 -c 'nc -l 8080'`.
* 使用 -k 参数可以在终端断开后继续保持连接.
* 传输文件:

    ```shell
    $ # server 端
    $ nc -l 8080 > file.txt
    
    $ # cline 端
    $ nc <IP> 8080 --send-only < data.txt
    ```
    
* 作为代理:

    ```shell
    $ # 单向转发
    $ nc -l <PORT> | nc <IP> <PORT>
    
    $ # 双向管道
    $ mkfifo 2way
    $ nc -l <PORT> 0<2way | nc <IP> <PORT> 1>2way
    ```
    
#### 参考
* [10 个例子教你学会 ncat (nc) 命令](https://linux.cn/article-9190-1.html)


## 网络模型

![TCP 流与报文](http://files-kyo.oss-cn-hongkong.aliyuncs.com/FqHQj_TybNiDuLzSb3bJx-DFaQsY.png)

![OSI 模型与 TCP/IP 模型对照](http://files-kyo.oss-cn-hongkong.aliyuncs.com/FimmYldrt6FXovwmltalWnYVIHuE.png)