# Security

## Note
遇到既需要加密又需要压缩的场景, 一定是先压缩再加密.


## Tools
* [cowrie/cowrie](https://github.com/cowrie/cowrie): 一个模拟的 SSH 服务器. 很多攻击者都是 SSH 登录, 你可以把这个软件在22端口启动, 真正的 SSH 服务器放在另一个端口. 黑客以为攻入了服务器, 其实进入的是一个虚拟系统, 然后会把他们的行为全部记录下来.