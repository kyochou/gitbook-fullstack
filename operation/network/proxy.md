# Proxy

## Tools

* [sshuttle/sshuttle](https://github.com/sshuttle/sshuttle)

    基于 SSH 的 VPN 代理. 可快速使用远程主机代理本地的所有的网络流量.
    
    ```shell
    sshuttle --dns -r username@sshserver 0/0
    ```
    
* [snail007/goproxy](https://github.com/snail007/goproxy)

    Proxy 是 golang 实现的高性能代理服务器, 支持正向代理, 反向代理, 透明代理, 内网穿透, TCP/UDP 端口映射, SSH 中转, TLS 加密传输, 协议转换, 防污染 DNS 代理.  

* [avwo/whistle](https://github.com/avwo/whistle)

    可拦截, 修改 `http(s)`, `websocket` 数据的代理工具. 使用时注意要安装 HTTPS 证书(不然无法正常拦截请求, 即使是 HTTP 协议).
    
* [cyfdecyf/cow](https://github.com/cyfdecyf/cow): 可自动为不可访问地址使用代理访问. COW can automatically identify blocked sites and use parent proxies to access.    
* 在线 IP 地址代理:
    * [全网代理IP](http://www.goubanjia.com/)



## FAQs
* 如何在 `sudo` 命令中使用环境变量中的代理设置(HTTP_PROXY, HTTPS_PROXY, FTP_PROXY, etc..)

    关键点是要将当前用户的环境变量代入到 `sudo` 环境中, 可通过 `visudo` 命令将要保留的环境变量添加到 `/etc/sudoers` 文件中:
    
    ```shell
    Defaults  env_keep += "http_proxy https_proxy ftp_proxy"
    ```
    
    参考 ['sudo gem install ...' behind a proxy](http://jacob.stanley.io/2010/10/27/sudo-gem-install-behind-a-proxy/)