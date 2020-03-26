# Nginx.conf


```nginx
server
{
    ## 跨域
    location / {
        add_header Access-Control-Allow-Origin *;
        if ($request_method = OPTIONS) {
            add_header Access-Control-Request-Method GET,POST,PUT,PATCH,DELETE;
            add_header Access-Control-Allow-Headers Content-Type,Accept,Authorization;
            return 200;
        }
    }
    
    ## 显示目录内容
    location / {
        autoindex on;
        autoindex_localtime on;
        autoindex_exact_size off;
        
        alias /var/log/supervisor;
        #root /var/log/supervisor;
    }
}
```