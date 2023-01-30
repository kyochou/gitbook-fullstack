# Logrotate
Logrotate 是一个日志文件管理工具. 用来对日志文件进行轮转, 压缩, 删除等操作.


## Supervisor
### supervisor 配置
/etc/supervisor/conf.d/app.conf:

```ini
[program:app]
redirect_stderr=true
stdout_logfile_maxbytes=0
```

### logrotate 配置
不使用 logrotate 默认的 cron 执行. 因此也不能将配置文件放在 `/etc/logrotate.d` 目录下.
注意需要将配置文件的所有者改为 `root`, 否则不会生效: 
```shell
sudo chown -R root:root /home/kyo/etc/logrotate.supervisor
```

/home/kyo/etc/logrotate.supervisor:
```ini
/var/log/supervisor/*.log {
  rotate 168 # 指定日志文件删除之前转储的次数
  copytruncate # 用于还在打开中的日志文件, 把当前日志备份并截断. 是先拷贝再清空的方式，拷贝和清空之间有一个时间差，可能会丢失部分日志数据。
  compress # 通过 gzip 压缩转储以后的日志
  delaycompress # 和compress 一起使用时, 转储的日志文件到下一次转储时才压缩
  missingok # 如果日志丢失, 不报错继续滚动下一个日志
  dateext # 使用当期日期作为命名格式
  dateyesterday # 使用昨天的日期
  dateformat .%Y%m%d # 按天切割
  #dateformat .%Y%m%d_%H # 按小时切割. need logrotate version gt 3.9.2

  ifempty # 即使日志文件为空文件也做轮转
  noolddir # 转储后的日志文件和当前日志文件放在同一个目录下
  postrotate # 在 logrotate 转储之后需要执行的指令. 必须独立成行
    t="$2.warning"
    egrep -v 'level=info|level=debug|level=trace|duplicate proto type registered' "$2" > "$t"
    [ -s "$t" ] || rm "$t"
  endscript
}
```

### cron 配置
`sudo cron -e` 添加 `00 00 * * * /usr/sbin/logrotate -f /home/kyo/etc/logrotate.supervisor`.

## Refs
* [logrotate/logrotate](https://github.com/logrotate/logrotate)
* [logrotate](https://wangchujiang.com/linux-command/c/logrotate.html)
* [CentOS 7下使用Logrotate管理日志](https://www.jianshu.com/p/6d3647f02437)