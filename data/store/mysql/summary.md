# MySQL

## Override
### MySQL 的逻辑架构图
[MySQL 的逻辑架构图](https://static001.geekbang.org/resource/image/0d/d9/0d2070e8f84c4801adbfa03bda1f98d9.png)

### 连接器
使用 `show processlist` 命令可以查看当前连接服务器的客户端信息.
建立连接的过程通常是比较复杂的. 对于需要经常操作数据库的程序而言, 应该尽量使用长连接. 
长连接会持续占用系统内存. 在 MySQL5.7 中, 可以在每次执行一个比较大的操作后, 通过执行 `mysql_reset_connection` 来重新初始化连接资源. 这个过程不需要重连和重新做权限验证, 但是会将连接恢复到刚刚创建完时的状态.

### 优化器
优化器是在表里面有多个索引的时候, 决定使用哪个索引; 或者在一个语句有多表关联(join)的时候, 决定各个表的连接顺序.



## Usage
* 数据导出/导入

    ```
      # 导出
      mysqldump -u -p db_name > file
      # 导入
      # 在要导入的 mysql 中创建数据库
      mysql -u -p db_name < file
    ```
* 创建用户/权限
    注意 MySQL 用户是由用户名和访问地址授权的.
    
    ```
      # 创建用户并赋予权限(主机名为 % 表示不限制访问地址)
      grant all on dedecms_test.* to 'dedecms'@'192.168.42.%' [identified by 'db_password'];
      # 删除用户
      drop user 'dedecms'@'192.168.42.%';
      # 刷新权限
      flush privileges;
    ```
    
## Replication(主从同步)
`server-id` 的取值范围是 1 到 4294967295(2**32 - 1). 为防止重复, `server-id` 可使用 IP 地址的后三位.

1. 修改 mysql 配置文件. 修改后重启服务.

    ```ini
    # master my.cnf
    server-id = 1
    log-bin = mysql-bin
    binlog_format = mixed
    innodb_flush_log_at_trx_commit=2
    sync_binlog=1
    ```
    
    ```ini
    # slave my.cnf
    server-id = 127001
    replicate-do-db=dbname
    ```

2. 手动同步已存在数据.
    
    ```shell
    # on master
    mysql > RESET MASTER;
    mysql > FLUSH TABLES WITH READ LOCK;
    mysql > SHOW MASTER STATUS;
    # 保存 MASTER_LOG_FILE, MASTER_LOG_POS 信息.
    # 不要关闭 mysql 命令行(不然锁会被释放). 打开新的 shell
    $ mysqldump -u root -p dbname > db.sql
    mysql > UNLOCK TABLES;
    mysql > GRANT REPLICATION SLAVE ON *.* TO 'repl'@'%' IDENTIFIED BY 'repl';
    
    # on slave
    mysql > STOP SLAVE;
    # 将 db.sql 自 master 传输到 slave
    mysql -uroot -p dbname < db.sql
    mysql > RESET SLAVE;
    # 在 5.6 版本中已不需要指定 master 的 position, mysql 会自动进行定位同步(添加 master_auto_position = 1)
    mysql > CHANGE MASTER TO MASTER_HOST='<master-ip>', MASTER_USER='repl', MASTER_PASSWORD='repl',  MASTER_LOG_FILE='<master-log-file>', MASTER_LOG_POS=<master-log-pos>;
    mysql > START SLAVE;
    mysql > SHOW SLAVE STATUS;
    # 数据无法同步时可试着重启 salve, 仍然无效的话可以 reset.
    mysql > stop slave;
    mysql > start slave;
    mysql > reset slave;
    ```

### Refs
* [MySQL5.6 GTID 新特性实践](http://cenalulu.github.io/mysql/mysql-5-6-gtid-basic/)
* [How to re-sync the Mysql DB if Master and slave have different database incase of Mysql replication?](https://stackoverflow.com/questions/2366018/how-to-re-sync-the-mysql-db-if-master-and-slave-have-different-database-incase-o#answer-3229580)
    
## Sites
* [http://www.innomysql.com/](http://www.innomysql.com/)


## Tools
* [mycli](https://www.mycli.net/)

    ```shell
    # centos 7.4
    sudo yum install python-pip python-devel
    sudo pip install backports.csv tabulate terminaltables
    sudo pip  --upgrade --force-reinstall  configobj    
    sudo pip install mycli
    ```