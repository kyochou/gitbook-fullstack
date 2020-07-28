# 日志

日志的作用有:
* 打印调试;
* 问题定位;
* 行为分析;


Trace 级别的日志只应该在开发环境中开启.   

## Level
* Fatal
    发生致命错误, 程序需要中断执行.
    
* Error
    需要引起人们注意的错误.
    大多数难以优雅处理的异常都属于 Error 范畴.
    
* Warn
    当前或未来潜在的问题.
    也可能是程序在处理某些任务时出现错误.
    
* Info
    交互操作或程序状态发生了变化. 它不会包含太多技术细节, 只是粗略的信息说明.
    
* Debug
    记录程序运行流程或用来定位问题的信息. 
    
* Trace(Verbose)
    用来帮助开发调试的更加具体的信息, 此级别在生产环境不应该开启.
    
    
## Practices

```shell
# 清空文件
sudo truncate -s 0 /var/log/**/*.log 

```
    
    
    

### Refs
* [日志的 5 个级别](http://www.infoq.com/cn/articles/five-levels-of-logging)
* [日志收集的“DNA”](https://mp.weixin.qq.com/s/ySJudWmuQfKRFY2snKIs1w)
