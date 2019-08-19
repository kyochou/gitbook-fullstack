# Develop

## 最佳实践
* 减少内存(如 `[]byte`)的分配, 尽量去复用它们.
    有两种方式进行复用:
    * `sync.Pool`
    * `slice = slice[:0]`

### Refs
* [fasthttp中运用哪些go优化技巧？](https://mp.weixin.qq.com/s/7zNw3nEWozArJLFVmTjn0A)    

## 命名
一般的, Go 中接口的命名以 `er` 为后缀, 如 `Reader`, `Writer`.

## Tools
### Lint
使用 [mgechev/revive](https://github.com/mgechev/revive#comment-annotations) 代替 golint.

```shell
# lint tool
go get -u github.com/mgechev/revive
```

### Dump
* [davecgh/go-spew](https://github.com/davecgh/go-spew): Implements a deep pretty printer for Go data structures to aid in debugging


### Debug
使用工具 [go-delve/delve](https://github.com/go-delve/delve).
启动调试服务(供远程 Debug 使用): `dlv attach $PID --headless --api-version=2 --log --listen=:12340`

#### VSCode

```json
// launch.json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Local",
      "type": "go",
      "request": "launch",
      "mode": "auto",
      "program": "${fileDirname}",
      "env": {},
      "args": []
    },
    {
      "name": "Remote",
      "type": "go",
      "request": "attach",
      "mode": "remote",
      "port": 12340,
      "host": "192.168.198.254",
    }
  ]
}

```

## Code Gen
* [fatih/gomodifytags](https://github.com/fatih/gomodifytags): 根据结构体生成 tags 注释代码.

### Refs
* [Go 终极指南：编写一个 Go 工具](https://www.jianshu.com/p/20b533c5c3f9?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)