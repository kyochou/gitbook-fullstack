# Memory

需要注意的是, GC 后回收的内存可能并不会立即还给操作系统.   


## MemStats   
相关字段含义参考 [MemStats](https://golang.org/pkg/runtime#MemStats).   

* `Alloc/HeapAlloc` 表示应用层代码正在使用的内存大小.   
* `Sys` 表示从操作系统申请的内存大小.   
* `HeapReleased` 是当前不在使用需要返还给操作系统(但可能还未返还)的内存大小.


## Refs
* [如何分析golang程序的内存使用情况](https://pengrl.com/p/24169/)
* [Go内存泄漏？不是那么简单!](https://colobu.com/2019/08/28/go-memory-leak-i-dont-think-so/)