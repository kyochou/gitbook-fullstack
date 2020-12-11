# Port
## 端口转发
### 通过 iptables

```bash
# 将 8080 转发到本地的 22222
iptables -A PREROUTING -t nat -p tcp --dport 8080 -j DNAT --to 127.0.0.1:22222
```

### 通过 Nginx 转发

```nginx
# 将 8080 转发到本地的 22222
# tengine 的 stream_sni 模块可实现为 server 添加 server_name 功能. https://tengine.taobao.org/document_cn/stream_sni_cn.html
stream {
    upstream proxy_name {
        server 127.0.0.1:22222;
    }
    server {
        listen 8080;
        proxy_pass proxy_name;
    }
}

```