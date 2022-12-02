# Crontab

## 命令格式
* `%` 会被认为是换行, 如果命令中包含 `%`, 需要使用 `\` 转义.   

## 计划格式
### Tools
* [crontab.guru](https://crontab.guru/): 在线的 crontab 计划计算工具.   


## FAQs
* `crontab -l`: 输出现有的 crontab 内容.
* 发送邮件:

    Linux 的 cron 服务发送执行结果需要 MTA.
    Crontab 的运行日志记录在 `/var/log/cron.log`, 开启 sendmail 服务(需要 MTA)后会给当前 Crontab 运行日志发送至 Crontab 指定的邮件.

* 日志中出现 `mailed xxx bytes of output but got status 0x004b#012` 错误:   
    原因在于 `postfix` 服务没有正常配置. 解决方法是:   
        1. 编辑 `/etc/postfix/main.cf` 文件, 把 `inet_interfaces = all` 这一行前面的注释符号取消;  
        2. 注释掉 `inet_interfaces = localhost`;  
        3. `service postfix start`;    

    参考 [crontab无法执行且(root) MAIL (mailed 54 bytes of output but got status 0x004b#012错误](https://blog.csdn.net/toopoo/article/details/104979615?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.nonecase)

        
* 在 Shell 中添加 crontab 记录:  

    ```bash
    # write out current crontab
    crontab -l > tmp_cron
    # echo new cron into cron file
    echo "00 09 * * 1-5 echo hello" >> tmp_cron
    # install new cron file
    crontab tmp_cron
    rm tmp_cron
    ```
    
    参考 [How to create a cron job using Bash automatically without the interactive editor?](https://stackoverflow.com/questions/878600/how-to-create-a-cron-job-using-bash-automatically-without-the-interactive-editor)