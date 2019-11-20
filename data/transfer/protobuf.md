# Protobuf
## Golang
使用 `github.com/golang/protobuf/proto` 包的 `Marshal`, `Unmarshal` 方法在 `[]byte` 与结构体之间转换. 不要使用 Protobuf 结构体的 `XXX_Unmarshal`, `XXX_Marshal` 方法, 此方法在有 Message 嵌套时会出现解析失败的情况:

```go
import "github.com/golang/protobuf/proto"

resp := &pb.Respone{
		Code: 1,
		Msg:  `error message`,
	}

// struct to bytes
bs, _ := proto.Marshal(resp)
// bytes to struct
r1 := &pb.Respone{}
proto.Unmarshal([]byte{8, 1, 18, 5, 101, 114, 114, 111, 114}, r1)
```

使用 [jsonpb](https://godoc.org/github.com/golang/protobuf/jsonpb) 可以的在 JSON 与 Protobuf 结构体之间转换:

```go
r := &pb.Respone{}
// string to protobuf object
jsonpb.UnmarshalString(`{"Code":1, "Msg":"error message"}`, r)
```
