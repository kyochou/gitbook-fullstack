# System

## Security
`iTerm` 每次修改系统数据(如 `cron`)都会出现是否可以执行的窗口, 此时需要给应用设置 `Full Disk Access` 权限(`System Preferences` -> `Security & Privacy` -> `Privacy` -> `Full Disk Access` -> `Add iTerm`). 如果需要在计划任务中执行系统相关的命令, 也要给 `/usr/sbin/cron` 赋予此权限.   

## FS
### Trash
在 `Finder` 中删除文件时, 系统会将文件的来源信息储存在 `.Trash/.DS_Store` 文件中. 所以通过将文件直接移动到 `.Trash` 目录中实现的删除是没有还原选项的.

## Network
* 修改默认的上网网卡(default gateway)

    [How to change the default gateway of a Mac OSX machine](https://apple.stackexchange.com/questions/33097/how-to-change-the-default-gateway-of-a-mac-osx-machine)

## Tools

* [sleepwatcher](https://www.bernhard-baehr.de/): 在系统进入 Sleep 和 Wake 状态时执行相关脚本. 

    ```shell
    mkdir -p /usr/local/sbin
    brew install sleepwatcher
    brew services restart sleepwatcher
    ps aux | grep sleepwatcher
    
    ```
    
    注意需要给相应脚本添加执行权限.

* [blueutil](https://github.com/toy/blueutil): 控制蓝牙.
* [brightness](https://github.com/nriley/brightness): 系统亮度控制的 CLI 工具. 亮度设置为 0 后可通过快捷键(Fn)把亮度调回来.
* 