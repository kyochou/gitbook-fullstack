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


## FAQs
* SSH 无法登录, 卡在 `expecting SSH2_MSG_KEX_ECDH_REPLY`

    修改网卡的 mtu 值为 1400 即可. 
    以太网的 MTU 默认是 1500, 而隧道的 MTU 值为 1400 左右, 比以太网的小. 因此, 以太网发出去的包就被拒绝了, 最终导致无法建立 SSH 连接.
    参考: [SSH 使用问题以及解决方案 (expecting SSH2_MSG_KEX_ECDH_REPLY)](https://github.com/johnnian/Blog/issues/44)
    
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