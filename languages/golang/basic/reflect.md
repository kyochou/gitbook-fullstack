# Reflect

```go

type (
	user struct {
		ID   uint64
		Info userinfo
	}

	userinfo struct {
		Name string
		Age  int
	}
)

func main() {

	s := user{}

	// 创建新的类型实例
	ref := reflect.New(reflect.TypeOf(s))
	ref.Elem().FieldByName(`ID`).SetUint(1)
	ref.Elem().FieldByName(`Info`).Set(reflect.ValueOf(userinfo{
		Name: `kyo`,
		Age:  18,
	}))
	fmt.Printf("%#v, %#v\n", s, ref.Interface())

	// 直接操作反射对象
	refPointer := reflect.Indirect(reflect.ValueOf(&s))
	refPointer.FieldByName(`ID`).SetUint(1)
	refPointer.FieldByName(`Info`).Set(reflect.ValueOf(userinfo{
		Name: `kyo`,
		Age:  18,
	}))
	fmt.Printf("%#v, %#v\n", s, refPointer.Interface())
}
```

## Refs
* [mitchellh/mapstructure](https://github.com/mitchellh/mapstructure): Go library for decoding generic map values into native Go structures.