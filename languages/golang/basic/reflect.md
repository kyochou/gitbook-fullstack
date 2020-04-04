# Reflect

运行时反射是程序在运行期间检查其自身结构的一种方式. `reflect` 包实现了运行时的反射能力. `reflect.TypeOf` 能获取类型信息, `reflect.ValueOf` 能获取数据的运行时表示.   
反射包中的所有方法基本都是围绕着 `Type` 和 `Value` 这两个类型设计的. 我们通过 `reflect.TypeOf` 和 `reflect.ValueOf` 可以将一个普通的变量转换成 `reflect`  包中提供的 `Type` 和 `Value`, 随后就可以使用反射包中的方法对它们进行复杂的操作了.      


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