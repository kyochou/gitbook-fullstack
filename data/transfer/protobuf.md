# Protobuf
## Golang
使用 Protobuf 结构体的 `XXX_Unmarshal`, `XXX_Marshal` 方法在 `byte[]` 与 Protobuf 结构体之间转换:

```go
resp := &pb.Respone{
		Code: 1,
		Msg:  `error message`,
	}

// struct to bytes
bs, _ := resp.XXX_Marshal(nil, true)

r1 := &pb.Respone{}
// bytes to struct
r1.XXX_Unmarshal([]byte{8, 1, 18, 5, 101, 114, 114, 111, 114})
```

使用 [jsonpb](https://godoc.org/github.com/golang/protobuf/jsonpb) 可以的在 JSON 与 Protobuf 结构体之间转换:

```go
r := &pb.Respone{}
// string to protobuf object
jsonpb.UnmarshalString(`{"Code":1, "Msg":"error message"}`, r)
```
