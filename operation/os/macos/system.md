# System

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