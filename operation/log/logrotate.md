# Logrotate
Logrotate 是一个日志文件管理工具. 用来对日志文件进行轮转, 压缩, 删除等操作.


## Supervisor
### supervisor 配置
/etc/supervisor/supervisord.conf:

```ini
[supervisord]
logfile_maxbytes=0
logfile_backups=0
```

### logrotate 配置
不使用 logrotate 默认的 cron 执行. 因此也不能将配置文件放在 `/etc/logrotate.d` 目录下.
注意需要将配置文件的所有者改为 `root`, 否则不会生效: 
```shell
sudo chown -R root:root /home/kyo/etc/logrotate.daily/supervisor
```

/home/kyo/etc/logrotate.daily/supervisor:
```ini
/var/log/supervisor/*.log {
	daily
	rotate 32
	copytruncate
	compress
	delaycompress
	missingok
	dateext
	dateyesterday
	dateformat -%Y%m%d
	ifempty
	noolddir
}
```

### cron 配置
`sudo cron -e` 添加 `00 00 * * * /usr/sbin/logrotate -f /home/kyo/etc/logrotate.daily/supervisor`.

## Refs
* [CentOS 7下使用Logrotate管理日志](https://www.jianshu.com/p/6d3647f02437)