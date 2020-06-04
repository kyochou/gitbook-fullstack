# Memory
## 内存管理
需要注意的是, GC 后回收的内存可能并不会立即返还给操作系统. 在启动程序时添加环境变量 `GODEBUG=madvdontneed=1` 可促使程序将不使用的内存尽快返还给操作系统(不推荐使用).   
Go 语言的内存分配器会根据申请分配的内存大小选择不同的处理逻辑, 运行时根据对象的大小将对象分成微对象(`(0, 16B)`), 小对象(`[16B, 32KB]`), 大对象(`(32KB, +∞`).   

### Refs
* [内存分配器](https://draveness.me/golang/docs/part3-runtime/ch07-memory/golang-memory-allocator/)   
* [图解Golang的内存分配](https://i6448038.github.io/2019/05/18/golang-mem/)


## MemStats   
相关字段含义参考 [MemStats](https://golang.org/pkg/runtime#MemStats).   

* `Alloc/HeapAlloc` 表示应用层代码正在使用的内存大小.   
* `Sys` 表示从操作系统申请的内存大小.   
* `HeapReleased` 是当前不在使用需要返还给操作系统(但可能还未返还)的内存大小.


### Refs
* [如何分析golang程序的内存使用情况](https://pengrl.com/p/24169/)
* [Go内存泄漏？不是那么简单!](https://colobu.com/2019/08/28/go-memory-leak-i-dont-think-so/)   

