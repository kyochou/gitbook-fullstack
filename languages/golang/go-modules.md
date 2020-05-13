# Go Modules

## Practices
运行 `go mod vendor` 命令时要注意, 没有在代码中使用到的库是不会被同步到 `vendor` 目录下的. 如果要同步可以在代码中使用如 `_ "github.com/nats-io/nats.go"` 先导入然后再同步.   


## Refs
* [Go modules：最小版本选择(Minimal Version Selection, MVS)](https://tonybai.com/2019/12/21/go-modules-minimal-version-selection/)