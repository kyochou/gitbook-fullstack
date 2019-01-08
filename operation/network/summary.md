# Network

## CLI

### Speed

使用 [sivel/speedtest-cli](https://github.com/sivel/speedtest-cli) 测试网络的上传和下载速度, 使用 [reorx/httpstat](https://github.com/reorx/httpstat) 测试网站的打开速度.

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

![TCP 流与报文](https://files-kyo.oss-cn-hongkong.aliyuncs.com/FqHQj_TybNiDuLzSb3bJx-DFaQsY.png)