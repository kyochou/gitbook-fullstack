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

## SSH Keys
* 将 OPENSSH 格式的私钥转换为 RAS 格式: [How-to : Convert OpenSSH private keys to RSA PEM](https://federicofr.wordpress.com/2019/01/02/how-to-convert-openssh-private-keys-to-rsa-pem/).

## SSHD
### sshd_config
DNS 反向解析用来屏蔽非法的 IP 访问请求. 如果不需要验证请求的 IP, 可在 `sshd_config` 文件中通过将 `UseDNS` 属性设置为 `no` 来关闭此功能, 以加快连接速度.

```properties
# 开启公钥验证
PubkeyAuthentication yes
# 不允许 root 帐号登录
PermitRootLogin no
# 不允许使用密码验证登录
PasswordAuthentication no
ChallengeResponseAuthentication no
# 修改默认端口
Port 10022
## 优化性能
UseDNS no
GSSAPIAuthentication no
```

### Refs
* [利用 ControlPersist 特性自动登陆 SSH 服务器](https://www.hi-linux.com/posts/39001.html)


## FAQs
* SSH 无法登录, 卡在 `expecting SSH2_MSG_KEX_ECDH_REPLY`

    禁用 IPv6: `networksetup -setv6off "Wi-Fi"`.
    结合 `ping` 和 `tracepath` 命令找出最佳的 MTU 值(一般不用改).
        
* 删除 SSH 登录痕迹

    查看用户登录信息的命令有 `w, last`(对应日志文件为 `/var/log/wtmp`), `lastb`(对应日志文件为 `/var/log/btmp`), `lastlog`(对应日志文件为 `/var/log/lastlog`), 这几个命令的输入可以使用工具 [re4lity/logtamper](https://github.com/re4lity/logtamper) 清除.
    
    ```shell
    #!/bin/bash

    ## 登录系统时执行此脚本可清除 w, last, lastb, lastlog 日志的输出
    ips=`sudo last | grep -i $USER | perl -F'\s+' -lane 'print $F[2]' | uniq`
    for ip in $ips
    do
      sudo python ~/.k_a_script/logtamper.py -m 1 -u $USER -i $ip
      sudo python ~/.k_a_script/logtamper.py -m 2 -u $USER -i $ip
    done
    sudo python ~/.k_a_script/logtamper.py -m 3 -u $USER  -i '' -t 'pts/0' -d `date "+%Y:%m:%d:%H:%M:%S"`

    ```
    
    记录 sshd 登录日志的文件有 `/var/log/secure` 和 systemd 系统日志, `/var/log/secure` 文件可直接编辑删除, systemd 产生的日志(sshd 服务的日志)则只能按日期来清除.
    
    参考: 
    * [HowTo: Clear or Remove Last Login History in Linux](https://www.shellhacks.com/clear-remove-last-login-history-linux/)
    * [Linux 入 * 侵分析（二）分析 SSH 登录日志](https://blog.51cto.com/winhe/2114533)