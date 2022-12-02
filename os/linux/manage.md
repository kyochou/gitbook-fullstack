# Manage
## Tools
* [Supervisor](http://supervisord.org/): A Process Control System.    
    使用 tail 命令可查看指定服务的输出.   
    子程序的状态:  
    ![](http://supervisord.org/_images/subprocess-transitions.png)   
    
    其中 `BACKOFF` 表示进程进入 `STARTING` 状态后, 由于马上就退出导致没能进入 `RUNNING` 状态. 一般在启动的进程为后台进程时出现(进程启动后马上进入后台运行).      

    