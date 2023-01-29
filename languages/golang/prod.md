# Prod
## Build
build 时添加参数 `-gcflags=all="-N -l" ` 以使生成的 coredump 文件中的变量可读.

## Run
### coredump
崩溃时生成 coredump 文件: `ulimit -c unlimited && GOTRACEBACK=crash ./main`.
ubuntu 默认生成 coredump 文件的目录为 `/var/lib/apport/coredump`.
使用 GDB 生成运行时程序的 coredump 文件: `sudo gcore <pid>`.

#### Refs
* [Core dump](https://wiki.archlinux.org/title/Core_dump)
* [Go: Debugging with Delve & Core Dumps](https://medium.com/a-journey-with-go/go-debugging-with-delve-core-dumps-384145b2e8d9)
* [Exploring Go core dumps](https://www.jetbrains.com/help/go/exploring-go-core-dumps.html)