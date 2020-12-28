# APT
## config

```json
# 设置代理
# /etc/apt/apt.conf.d/90proxy
Acquire {
  HTTP::proxy "http://127.0.0.1:8080";
  HTTPS::proxy "http://127.0.0.1:8080";
}
```