# Nginx.conf


```nginx
server
{
    location / {
        # 跨域
        if ($request_method = OPTIONS) {
            add_header Access-Control-Request-Method GET;
            add_header Access-Control-Allow-Origin *;
            return 200;
        }
    }
}
```