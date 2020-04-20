# Runtime

gortinfo.sh: 用来收集 Go 程序的运行时信息.  

```bash
#!/bin/bash

[ $# -ne 2 ] && { echo "Usage: $0 pid port"; exit 1; }

pid=$1
port=$2

dir=go-runtime-info_${port}_`date +%Y-%m-%dT%H:%M:%S%z`
mkdir $dir
url=127.0.0.1:${port}/debug/pprof

wget $url/allocs -O $dir/allocs.bin
wget $url/allocs?debug=1 -O $dir/allocs.txt

wget $url/block -O $dir/block.bin
wget $url/block?debug=1 -O $dir/block.txt

wget $url/goroutine -O $dir/goroutine.bin
wget $url/goroutine?debug=1 -O $dir/goroutine.txt
wget $url/goroutine?debug=2 -O $dir/goroutine-full.txt

wget $url/heap -O $dir/heap.bin
wget $url/heap?debug=1 -O $dir/heap.txt

wget $url/mutex -O $dir/mutex.bin
wget $url/mutex?debug=1 -O $dir/mutex.txt

wget $url/threadcreate -O $dir/threadcreate.bin
wget $url/threadcreate?debug=1 -O $dir/threadcreate.txt

wget $url/profile -O $dir/profile.bin
wget $url/trace -O $dir/trace.bin

sudo gcore -o $dir/core $pid

```
