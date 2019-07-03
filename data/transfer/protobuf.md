# Protobuf
## Golang
使用 Protobuf 结构体的 `XXX
使用 [jsonpb](https://godoc.org/github.com/golang/protobuf/jsonpb) 可以的在 JSON 与 Protobuf 结构体之间转换:

```go
r := &pb.Respone{}
// string to protobuf object
jsonpb.UnmarshalString(`{"Code":1, "Msg":"error message"}`, r)
```
