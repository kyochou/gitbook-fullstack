# System

## FS
### Trash
在 `Finder` 中删除文件时, 系统会将文件的来源信息储存在 `.Trash/.DS_Store` 文件中. 所以通过将文件直接移动到 `.Trash` 目录中实现的删除是没有还原选项的.

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