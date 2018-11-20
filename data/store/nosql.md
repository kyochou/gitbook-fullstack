# NoSQL

## MongoDB
### Tools
* [docker-mongo](https://github.com/docker-library/docs/blob/master/mongo/README.md): 基于 Docker 的 Mongo 服务.
* [adminMongo](https://github.com/mrvautin/adminMongo): 基于 Web 界面的 mongo 管理工具.

### FAQs
* 通过 shell 连接提示 "login failed"

    连接时指定 authSource 为 admin. 
    ```shell
    $ mongo mongodb://user:pass@127.0.0.1:27017/yourcollection?authSource=admin
    ```
    参考 [Unable to set up default credentials using environment variables](https://github.com/docker-library/mongo/issues/263).