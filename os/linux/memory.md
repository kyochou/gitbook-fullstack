# Memory

虚拟内存(Virtual Memory, VIRT): 表示程序运行中可以访问的最大内存空间.  
驻留内存(Resident Moemory, RES): 表示程序实际使用到的内存空间.   
SHR 表示进程运行过程中占用的共享内存空间(如程序依赖的动态链接库所占的内存)的大小. `RES-SHR` 的值即为程序独占内存空间的大小.


## free
使用 `free -h` 命令可查看系统内存使用情况. 系统的实际可用内存为 `available` 列显示的值.   


