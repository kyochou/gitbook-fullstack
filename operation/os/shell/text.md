# 文本处理

## 输出列

```shell
# 输出登录 IP
last | grep kyo | perl -F'\s+' -lanE'print $F[2]'
last | grep kyo | tr -s ' ' | cut -d ' ' -f 3
last | grep kyo | awk '{print $3}'
```