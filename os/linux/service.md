# Service

## Mail
因 `ssmtp` 和 `postfix` 都使用 `mail` 命令发送邮件, 以至于配置上会起冲突. 一个解决方案是使用 `postfix` 作为本地的邮件服务, 使用 `mutt` 和外界通信. 

```shell
sudo apt-get install postfix mailutils mutt -y
# postfix 用来作为本地服务, 不需要配置, 使用默认配置即可. 
```

```ini
## vi ~/.muttrc

# About Me
set from = "noreply_kyo@163.com"
set realname = "kyo@pi"

# My credentials
set ssl_starttls=yes
set ssl_force_tls=yes
set smtp_url = "smtps://noreply_kyo@163.com@smtp.163.com:587/"
set smtp_pass = "<password>"

```

```shell
# 测试
echo "test" | mailx -s "mutt" live.kyo@gmail.com
```


### Postfix

```shell
sudo apt-get install ssmtp mailutils -y
```

```ini
# sudo vi /etc/ssmtp/ssmtp.conf
root=postmaster
mailhub=smtp.163.com
hostname=raspi
FromLineOverride=Yes
UseSTARTTLS=YES
UseTLS=YES
AuthUser=noreply_kyo@163.com
AuthPass=<password>
```

```shell
# 保护明文密码
sudo chmod 660 /etc/ssmtp/ssmtp.conf

## 注意, 有的邮箱服务要求邮件的发送者必须和配置的真实发送者相同才能发送成功(发送报错: 553 Mail from must equal authorized user)
# 一种方法是使用 -a 标志重写发送者(配置中 FromLineOverride=Yes 要设置)
echo "Hello world" | mail -s "Test Email" -a "From: noreply_kyo@163.com" live.kyo@gmail.com
# 也可以在文件 /etc/ssmtp/revaliases 中为用户设置对应的发送者 
echo -e "root: noreply_kyo@163.com\nkyo: noreply_kyo@163.com" | sudo tee -a /etc/ssmtp/revaliases 
echo "Hello world" | mail -s "Test Email" live.kyo@gmail.com
```
