# File

## 合并
```shell
# 多个文件合并成一个
echo a.txt b.txt > c.txt
# 显示最后 9 行
tail -n 9 file
# 显示自第 9 行开始, 后面的所有行
tail -n +1000 file
# 显示前面 9 行
head -n 9 file
# 显示中间行(第 5 至第 10 行)
sed -n '5,10p' file
```

## 权限
### 用户
Linux 文件的访问者有三种身份:
* u: owner, 拥有者.
* g: group, 相同组的用户.
* o: other, 其他用户.

### 标志
* r: 读.
* w: 写.
* x: 执行.
* s: 此权限会占据 owner 用户的执行标志位. 可使文件在执行时具有文件所有者的权限, 相当于以文件所有者的身体执行文件.

    典型的文件是 passwd. 如果一般用户执行该文件, 则在执行过程中, 该文件可以获得 root 权限, 从而可以更改用户密码.
    
    ```shell
    ls -la /usr/bin/passwd
    -rwsr-xr-x 1 root root 54256 May 17  2017 /usr/bin/passwd
    ```
    
    可通过命令 `chmod a+s file` 设置此标志.
    此标志位在 SELinux 中默认是不起作用的, 通过 `visudo` 将要执行的命令和用户添加到 sudo 列表中, 然后在执行命令前添加 `sudo` 前缀, 也可实现同等的效果:
    ```ini
    # visudo
    username  ALL=(ALL) NOPASSWD: /path/to/sciprt.sh
    ```
    
    ```shell
    # with user username
    sudo /path/to/sciprt.sh
    ```
    
* t: 此权限会占据 other 用户的执行标志位. 用于设置一个目录可被所有人写入, 但只能删除自己创建的文件. 通过命令 `chmod a+t dir` 来设置此标志.

### 属性
* i: 不可修改(readonly). 使用命令 `sudo chattr +i file` 设置此属性.
* a: 只可追加. 使用命令 `sudo chattr +a file` 设置此属性.
