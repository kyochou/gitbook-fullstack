# DLV

## core
进入 dlv 环境的命令为 `dlv core <binfile> <corefile>`.   
在 dlv 中, 使用 `grs` 命令显示所有的 Goroutine, 使用 `gr <goroutine_id>` 命令进入到指定的 Goroutine, 在 Goroutine 中使用 `stack` 命令显示当前栈, 然后使用 `frame <frame_id>` 命令进入栈的指定帧, 进入指定帧后, 就可以使用 `locals`, `p` 等命令查看变量的值了.    


## Refs
* [go-delve/delve](https://github.com/go-delve/delve)