# Nginx

Nginx 的组成:
* 二进制可执行文件.
* nginx.conf 配置文件.
* access.log 访问日志.
* error.log 错误日志. 

版本: 与 Linux 内核版本一样, 偶数结尾的版本表示稳定版.


## 源码
* 将源码中 `contrib/vim/` 目录下的文件复制到 `~/.vim/` 目录下, 可以使文件 nginx.conf 中的关键字高亮显示(只能高亮显示这一个配置文件).

    ```shell
    cp -r contrib/vim/* ~/.vim/
    ```


## OpenResty