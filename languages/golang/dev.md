# Develop

## 最佳实践
* 减少内存(如 `[]byte`)的分配, 尽量去复用它们.
    有两种方式进行复用:
    * `sync.Pool`
    * `slice = slice[:0]`

### Refs
* [fasthttp中运用哪些go优化技巧？](https://mp.weixin.qq.com/s/7zNw3nEWozArJLFVmTjn0A)    


## Go module
* 设置代理:

    ```shell
    # 默认开启全局代理 
    export GOPROXY=https://goproxy.cn,direct
    # 指定不使代理的地址
    export GOPRIVATE=.gitlab.com
    ```
    
* 使用本地目录代替远程库:

    ```go
    // go.mod
    require (
    	github.com/lonng/nano v0.0.0 // 版本指定为 v0.0.0
    )
    // 使用 replace 指定本地路径
    replace github.com/lonng/nano => /Users/kyo/go/src/github.com/lonng/nano
    ```
    
### Refs
[Go module 再回顾](https://colobu.com/2019/09/23/review-go-module-again/?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)    

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