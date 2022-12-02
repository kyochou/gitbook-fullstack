# Process

## 进程状态
### zombie
僵尸进程是一个早已死亡的进程, 但在进程表(process table)中仍占了一个位置(slot).   
可使用 `ps aux | grep -w 'Z'` 命令找出僵尸进程, 想要结束僵尸进程, 只需 kill 掉其父进程即可.    
