# Security

## Note
遇到既需要加密又需要压缩的场景, 一定是先压缩再加密.

## Shell
**所有带有敏感信息的命令在执行时一定要在其前面添加空格, 以防止被记录进 history 日志中**. 

## Tools
* [cowrie/cowrie](https://github.com/cowrie/cowrie): 一个模拟的 SSH 服务器. 很多攻击者都是 SSH 登录, 你可以把这个软件在22端口启动, 真正的 SSH 服务器放在另一个端口. 黑客以为攻入了服务器, 其实进入的是一个虚拟系统, 然后会把他们的行为全部记录下来.
* [FiloSottile/age](https://github.com/FiloSottile/age): A simple, modern and secure encryption tool with small explicit keys, no config options, and UNIX-style composability. 一个命令行工具, 使用公钥/私钥对文件进行解密和加密, 用法很简单.  
* [simplelogin](https://simplelogin.io/): 一个开源的邮箱别名服务, 提供真实邮箱的别名, 可以接收和回复邮件, 所有邮件都会转到你的真实邮箱.  