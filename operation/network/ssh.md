# SSH

## .ssh/config

```ini
Host *
StrictHostKeyChecking no
ServerAliveInterval 59
## 加速
Compression yes
# 共享连接
ControlMaster auto
ControlPath /tmp/ssh_mux_%h_%p_%r
ControlPersist yes

```

### Refs
* [利用 ControlPersist 特性自动登陆 SSH 服务器](https://www.hi-linux.com/posts/39001.html)