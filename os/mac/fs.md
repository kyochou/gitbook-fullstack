# File System

## 访问 ext 系统
```shell
# 安装后有可能会要求重启电脑
brew install macfuse --cask
brew install ext2fuse
# 如果报 "requires closed-source macFUSE" 错误则执行 vi `brew formula ext2fuse`, 将这个错误相关的代码注释掉.

# 查看要挂载的 block
diskutil list 
# 以读写权限挂载
sudo ext4fuse /dev/disk3s1 ~/tmp/ext4_disk3s1 -o allow_other
```

### Refs
* [How to mount EXT4 disk on 10.15?](https://apple.stackexchange.com/questions/381278/how-to-mount-ext4-disk-on-10-15)
* [HOW TO MOUNT AN EXT PARTITION ON A MAC](https://hackmylinux.com/2018/02/18/how-to-mount-and-read-a-linux-partition-on-a-mac-ext2-ext3-ext4/)
* [【解决】brew无法安装三方库的问题：ifuse has been disabled because it requires closed-source macFUSE](https://blog.csdn.net/qq_27806965/article/details/122842894)