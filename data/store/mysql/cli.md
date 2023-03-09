# CLI

## conf
* `pager`
    mysql 的执行结果会发送给 `pager` 属性指定的命令执行. 比如要分页显示结果可以将  `pager` 的值设置为 `less`. 
    ```shell
    # connect mysql
    mysql --pager=less
    # mysql cli
    pager less;
    ```
    但实际上, 可以将 `pager` 属性设置为任何你想执行的命令. 如 `more`, `cat > /dev/null`, `md5sum` 等等. 

## character
* 在 mysql 中查看当前数据库编码: `show variables like 'character%';`.
* 在 `my.cnf` 中修改字符相关设置: 
    ```ini
        ## sudo vi /etc/mysql/my.cnf
        [mysqld]
        default-character-set=utf8
        # 设定连接 mysql 数据库时使用 utf8 编码
        init_connect='SET NAMES utf8'
    
        [client]
        default-character-set=utf8
    
```



## FAQs
* Mysql 无法远程连接:
    * 通过 `netstat -ano | grep 3306` 命令查看 mysqld 所监听的端口是本地(127.0.0.1) 还是所有(0.0.0.0).
    * 检查 my.cnf 文件中 `bind-address` 的值是否为 localhost(只有本地可以访问), 可以直接将其注释掉.
* 
