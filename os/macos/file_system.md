# FileSystem

## 访问 ext 系统
```shell
# 安装后有可能会要求重启电脑
brew install macfuse --cask
# 如果报 "requires closed-source macFUSE" 错误则执行 vi `brew formula macfuse`, 将这个错误相关的代码注释掉. 

# ext4fuse 只支持只读模式, fuse-ext2 支持读写
# 参考 https://github.com/alperakcan/fuse-ext2 安装 fuse-ext2


# 查看要挂载的 block
diskutil list 
# 以读写权限挂载
fuse-ext2 /dev/disk2s1 ~/tmp/disk2s1 -o rw+
```

### Refs
* [【解决】brew无法安装三方库的问题：ifuse has been disabled because it requires closed-source macFUSE](https://blog.csdn.net/qq_27806965/article/details/122842894)