# System

## FS
### Trash
在 `Finder` 中删除文件时, 系统会将文件的来源信息储存在 `.Trash/.DS_Store` 文件中. 所以通过将文件直接移动到 `.Trash` 目录中实现的删除是没有还原选项的.

## Network
* 修改默认的上网网卡(default gateway)

    [How to change the default gateway of a Mac OSX machine](https://apple.stackexchange.com/questions/33097/how-to-change-the-default-gateway-of-a-mac-osx-machine)

## Tools

* sleepwatcher: 在系统进入 Sleep 和 Wake 状态时执行相关脚本. 

    ```shell
    mkdir -p /usr/local/sbin
    brew install sleepwatcher
    brew services restart sleepwatcher
    ps aux | grep sleepwatcher
    
    ```
    
    注意需要给相应脚本添加执行权限.

* blueutil: 控制蓝牙. `brew install blueutil`.