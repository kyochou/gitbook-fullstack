# Nginx.conf


```nginx
http {
# 配置请求次数限制. 每IP($binary_remote_addr)每URI($uri)每秒允许请求 2 次(2r/s).
limit_req_zone "$binary_remote_addr$uri" zone=limitreq:10m rate=2r/s;
# 请求次数溢出返回码.
limit_req_status 429;


    server
    {
    
        # 开启请求限制. 注意 limit_req 在与 rerwite, tryfiles 指令在一起使用时可能会有问题. 最好不要放在 location 中. 
        limit_req   zone=limitreq nodelay;
                
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

}
```