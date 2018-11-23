# MySQL
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

