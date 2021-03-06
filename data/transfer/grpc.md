# gRPC
## What is
gRPC 是一个高性能, 开源和通用的 RPC 框架, 面向移动和 HTTP/2 设计.
gRPC 帮你解决了不同语言及环境间通信的复杂性.
在 gRPC 里客户端应用可以像调用本地对象一样直接调用另一台不同机器上服务端应用的方法, 可以更容易地创建分布式应用和服务.
其理念为: 定义一个服务, 指定其能够被远程调用的方法(包含参数和返回类型). 在服务端实现这个接口, 并运行一个 gRPC 服务器来处理客户端调用. 在客户端拥有一个存根, 其拥有与服务端一样的方法以供调用.

我们使用 protocol buffers 接口定义语言来定义服务方法, 用 protocol buffer 来定义参数和返回类型. 客户端和服务端均使用服务定义生成的接口代码.

### 服务定义
gRPC 允许你定义四类服务方法:
* 单向 RPC: 即客户端发送一个请求给服务端, 从服务端获取一个应答, 就像一次普通的函数调用.
* 服务端流式 RPC: 即客户端发送一个请求给服务端, 可获取一个数据流用来读取一系列消息. 客户端从返回的数据流里一直读取直到没有更多消息为止.
* 客户端流式 RPC: 即客户端用提供的一个数据流写入并发送一系列消息给服务端. 一旦客户端完成消息写入, 就等待服务端读取这些消息并返回应答.
* 双向流式 RPC: 即两边都可以分别通过一个读写数据流来发送一系列消息. 这两个数据流操作是相互独立的, 所以客户端和服务端能按其希望的任意顺序读写.

### 插件机制
gRPC 提供了可插拔的插件机制, 或者说是拦截器机制, 以对每一次 RPC 请求进行拦截.
基于此可方便的实现如身份验证, 负载均衡, 健康检查等功能.

### gRPC生态体系
* [grpc-ecosystem/awesome-grpc](https://github.com/grpc-ecosystem/awesome-grpc): A curated list of useful resources for gRPC.
* grpc-opentracing: 查看完整的链路调用环节.
* grpc-promethus: 对 gRPC 服务进行监控, 并将监控数据存储到 prometheus 中. 统计的指标包括: 发起了多少个请求, 接收到了多少个响应, 响应延迟等.
* grpc-gateway: 对于 gRPC 不支持的语言, 可使用 grpc-gateway 进行反向代理, 将 Restful JSON API 请求转换为 gRPC 请求.
* nginx, haproxy 等也提供了对 gRPC 的支持.

### Refs
* [漫谈gRPC](https://mp.weixin.qq.com/s/ah9gdutZueCxbqjrWVhiQg)


## Usage
### Tools
    
```shell
brew tap grpc/grpc
# 代码生成
brew install protobuf
# CLI 客户端
# brew install grpc
brew install grpcurl
```

#### grpc_cli
    
```shell
$ # 显示所有可用服务
$ grpc_cli ls <IP>:<PORT>
$ # 健康检查
$ grpc_cli call <IP>:<PORT> grpc.health.v1.Health.Check 'service:"<Your-Service>"'
```


#### grpcurl
[fullstorydev/grpcurl](https://github.com/fullstorydev/grpcurl)

```shell
# list servers
grpcurl -v -plaintext -proto api/proto/rpc/rpc.proto  34.92.222.179:61011 list
# request method
grpcurl -v -plaintext -proto api/proto/rpc/rpc.proto  34.92.222.179:61011 rpc.Center/RegisterGame
```

#### evans
[ktr0731/evans](https://github.com/ktr0731/evans): more expressive universal gRPC client .

## Golang

```shell
# download protobuf at https://github.com/protocolbuffers/protobuf/releases
# 安装 golang 代码生成插件 protoc-gen-go, 默认会被安装到 $GOPATH/bin 下
go get -d -u github.com/golang/protobuf/protoc-gen-go
# 生成 go 代码文件
# PROTOPATH 为 proto 文件所在目录
protoc --plugin=${GOPATH}/bin/protoc-gen-go --proto_path=${PROTOPATH} --go_out=plugins=grpc:${PROTOPATH} ${PROTOPATH}/*.proto
```

### FAQs
* 使用 `option go_package` 指定生成代码的包路径. 参考 [How to generate right Import paths #257](https://github.com/gogo/protobuf/issues/257).

### Tools
* [grpc-ecosystem/go-grpc-middleware](https://github.com/grpc-ecosystem/go-grpc-middleware): Golang gRPC Middlewares: interceptor chaining, auth, logging, retries and more.
* [grpc-ecosystem/grpc-gateway](https://github.com/grpc-ecosystem/grpc-gateway): gRPC to JSON proxy generator following the gRPC HTTP spec.
* [gogo/protobuf](https://github.com/gogo/protobuf): Protocol Buffers for Go with Gadgets.

### Refs
* [从实践到原理，带你参透 gRPC](https://github.com/EDDYCJY/blog/blob/master/golang/gRPC/2019-06-28-talking-grpc.md)
* [golang 网络框架之 grpc](http://www.hatlonely.com/2018/02/03/golang-%E7%BD%91%E7%BB%9C%E6%A1%86%E6%9E%B6%E4%B9%8B-grpc/)
* [grpc-go/examples](https://github.com/grpc/grpc-go/tree/master/examples/)
* [Writing gRPC Interceptors in Go](https://medium.com/@shijuvar/writing-grpc-interceptors-in-go-bf3e7671fe48)
    
## Java
1. 使用 springboot 做为基础框架, 安装 [grpc-spring-boot-starter](https://github.com/LogNet/grpc-spring-boot-starter).
2. 配置自动生成 stub: [protobuf-gradle-plugin](https://github.com/google/protobuf-gradle-plugin).
3. 在配置文件中将 `enableReflection` 设置为 true 以供 gRPC CLI 查询: [server-reflection-tutorial.md](https://github.com/grpc/grpc-java/blob/master/documentation/server-reflection-tutorial.md)
4. 编写你自己的 proto 文件及实现.
5. 开启服务: 在项目根目录执行命令 `./gradlew build && java -jar build/libs/<your-appname>.jar`.

## Resources
* [gRPC 官方文档中文版](https://doc.oschina.net/grpc)
* [Protobuf3 语法指南](https://colobu.com/2017/03/16/Protobuf3-language-guide/)
* [grpc.io](https://grpc.io/)
* [protocol-buffers](https://developers.google.com/protocol-buffers/)