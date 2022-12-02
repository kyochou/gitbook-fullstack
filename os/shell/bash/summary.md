# Bash

## ManPage
### Pipelines

管道中的命令会在独立的进程中执行. 这意味着他们之间的 `env` 是不共享的.   
> Each command in a pipeline is executed as a separate process (i.e., in a subshell).