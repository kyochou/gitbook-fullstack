# Proxy

## Server
* [shadowsocksr-native](https://github.com/ShadowsocksR-Live/shadowsocksr-native): ShadowsocksR (SSR) native implementation for all platforms, GFW terminator. SSR 源码.    


## Client
### ssh

```shell
# 使用 ssh 命令做为 socks5 代理 
ssh -q -N -C -D 19911 proxy
```

#### Refs
* [Create a SOCKS proxy on a Linux server with SSH to bypass content filters](https://ma.ttias.be/socks-proxy-linux-ssh-bypass-content-filters/)

### polipo
用于将 socks 代理转换为 http. 

```ini
socksParentProxy = 127.0.0.1:19911
socksProxyType = socks5

proxyAddress = 0.0.0.0
proxyPort = 19999
```


### V2ray
* [yanue/V2rayU](https://github.com/yanue/V2rayU): 基于 v2ray 核心的 mac 版客户端, 支持 vmess, shadowsocks, socks5 等服务协议.   

## Tools
* [inlets/inlets](https://github.com/inlets/inlets): Expose your local endpoints to the Internet or to another network, traversing firewalls and NAT.
* [hmgle/graftcp](https://github.com/hmgle/graftcp): graftcp 可以把任何指定程序(应用程序, 脚本, shell 等)的 TCP 连接重定向到 SOCKS5 或 HTTP 代理. 
* [yrutschle/sslh](https://github.com/yrutschle/sslh): Applicative Protocol Multiplexer (e.g. share SSH and HTTPS on the same port). 端口复用软件. 参考: [公网端口不够用，用这款神器轻松搞定它！](https://mp.weixin.qq.com/s?__biz=MzI3MTI2NzkxMA==&mid=2247489038&idx=1&sn=c9856cedabf74ccbffbca451f03fe05a)

* [txthinking/brook](https://github.com/txthinking/brook): Brook is a cross-platform(Linux/MacOS/Windows/Android/iOS) proxy/vpn software.

* [sshuttle/sshuttle](https://github.com/sshuttle/sshuttle)

    基于 SSH 的 VPN 代理. 可快速使用远程主机代理本地的所有的网络流量.
    
    ```shell
    sshuttle --dns -N -r username@sshserver 0/0
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
    
## Refs
* [yangchuansheng/love-gfw](https://github.com/yangchuansheng/love-gfw): gfw 综合解决方案.