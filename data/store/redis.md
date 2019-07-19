# Redis

## FAQs
* 使用正则删除 key:
    
    ```shell
    redis-cli --scan --pattern users:* | xargs redis-cli del 
    # redis >= 4.0
    redis-cli --scan --pattern users:* | xargs redis-cli unlink
    
