# Nginx.conf


```nginx
server
{
    location / {
        # 跨域
        add_header Access-Control-Allow-Origin *;
        if ($request_method = OPTIONS) {
            add_header Access-Control-Request-Method GET,POST,PUT,PATCH,DELETE;
            add_header Access-Control-Allow-Headers Content-Type,Accept,Authorization;
            return 200;
        }
    }
}
```