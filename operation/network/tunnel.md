# Tunnels

## 内网穿透
## Tools
* [Serveo](serveo.net): 基于 `ssh` 命令, 无需安装. 方便快捷. 参考 [无需安装，仅需 1 条指令，秒实现内网穿透的神器，你有用过吗？](https://mp.weixin.qq.com/s/UiS30NyGaeh0e2GyB3j2yQ).
* [frp](https://github.com/fatedier/frp): A fast reverse proxy to help you expose a local server behind a NAT or firewall to the internet.
    
    
### Ngrok
使用 ngrok 前, 试用了 n2n, 发现在传输的字节数变多时就无响应了. 放弃.

#### 安装
如果不需要使用自己的域名可以直接使用[在线服务](https://ngrok.com/), 简单方便.
想使用自己的域名, 只能下载源码编译安装. 具体过程可参考 [自行编译ngrok服务端客户端，替代花生壳，跨平台](http://ekan001.com/article/38). 需要注意的是, ngrok 的服务端和客户端都是根据证书生成的, 连接时需要进行验证, 编译时使用的证书要妥善保管.

#### 使用 ngrok 进行域名绑定
* 添加域名解析时, 需要将子域名及子域名下的泛解析域名的 A 记录(如 dev 及 *.dev)同时指向到主机上.

#### 使用 ngrok 进行 ssh 穿透登录
  1. 需要指定参数 proto 为 tcp.
  2. 如果需要使用固定的登录端口, 则需在 config 文件中指定 remote_port 参数. 如:
    
    ```
    tunnels:
      ssh:
        remote_port: 10022
          proto:
            tcp: 22
    ```               
    
#### Refs
* [Enable setting the remote port for non-HTTP services](https://github.com/inconshreveable/ngrok/issues/42)
* [ssh login permission denied](https://github.com/inconshreveable/ngrok/issues/145)
* [关于Ngrok的一些思考](http://my.oschina.net/atanl/blog/371556)