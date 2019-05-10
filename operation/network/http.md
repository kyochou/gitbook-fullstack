# HTTP

## HTTPS
### Tools
* [FiloSottile/mkcert](https://github.com/FiloSottile/mkcert): 本地 HTTPS 支持. mkcert is a simple tool for making locally-trusted development certificates. It requires no configuration.

## CA
CA, Catificate Authority, 它的作用就是提供证书以加强服务端和客户端之间信息交互的安全性, 以及证书运维相关服务.
证书类型:
* DV: domain validated, 域名验证证书.
* OV: organization validated, 组织验证证书.
* EV: extended validation, 扩展验证证书.


### TLS
传输层安全性协议(Transport Layer Security, TLS)及其前身安全套接层(Secure Sockets Layer, SSL)是一种安全协议, 目的是为互联网通信提供安全及数据完整性保障.
TLS 通讯过程:
1. 验证身份.
2. 达成安全套件共识.
3. 传递密钥.
4. 加密通讯.
