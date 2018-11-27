# Crontab
## FAQs
* `crontab -l`: 输出现有的 crontab 内容.
* 发送邮件:

    Linux 的 cron 服务发送执行结果需要 MTA.
    Crontab 的运行日志记录在 `/var/log/cron.log`, 开启 sendmail 服务(需要 MTA)后会给当前 Crontab 运行日志发送至 Crontab 指定的邮件.

## 命令格式
* `%` 会被认为是换行, 如果命令中包含 `%`, 需要使用 `\` 转义.

## 计划格式
### Tools
* [crontab.guru](https://crontab.guru/): 在线的 crontab 计划计算工具.