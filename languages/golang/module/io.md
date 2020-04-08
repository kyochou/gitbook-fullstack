# IO

## 概述

* `io` 包为 IO 原语(I/O primitives)提供基本的接口.
* `io/ioutil` 包封装了一些实用的 I/O 函数.
* `fmt` 包实现了格式化 I/O, 类似 C 语言的 `printf` 和 `scanf`.
* `bufio` 包实现了带缓冲的 I/O.

## IO 包
在另有声明之前不该假定它们的并行执行是安全的.

## ioutil 包
`ReadDir` 函数读取目录并返回排好序的文件和子目录名.
`ReadFile`, `WriteFile` 函数用于读写文件.
`TempDir`, `TempFile` 函数用于创建临时目录和文件. 需要自己负责删除.

## fmt 包
* Stringer 接口: 如果格式化输出某种类型的值, 只要它实现了 `String() string` 方法, 就会调用此方法进行处理.

