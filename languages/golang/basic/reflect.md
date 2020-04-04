# Reflect

运行时反射是程序在运行期间检查其自身结构的一种方式. `reflect` 包实现了运行时的反射能力. `reflect.TypeOf` 能获取类型信息, `reflect.ValueOf` 能获取数据的运行时表示. `reflect` 包中的所有方法基本都是围绕着 `Type` 和 `Value` 这两个类型设计的.如果我们知道了一个变量的类型和值, 就意味着知道了这个变量的全部信息.      
   
要通过反射修改一个变量的值需要:
1. 调用 `reflect.ValueOf` 函数获取变量指针;  
2. 调用 `reflect.Value.Elem` 方法获取指针指向的变量;   
3. 调用 `reflect.Value.Set...` 方法更新变量的值;   

由于 Go 语言的函数调用都是值传递的, 所以我们只能先获取指针对应的 `reflect.Value`, 再通过 `reflect.Value.Elem` 方法迂回的方式得到可以被设置的变量. 我们通过如下所示的代码理解这个过程:

```go
// 如果不能直接操作 i 变量修改其持有的值, 我们就只能获取 i 变量所在地址并使用 *v 修改所在地址中的值.
i := 1
v := &i
*v = 10
// 即 *&i = 10

```

当我们想要将一个变量转换成反射对象时, Go 语言会在编译期间完成类型转换的工作, 将变量的类型和值转换成了 `interface{}` 并等待运行期间使用 `reflect` 包获取接口中存储的信息.   

`reflect.Value.Kind` 方法可以返回反射对象的类型, 比如判断一个反射对象是否为函数:

```go
v := reflect.ValueOf(Add)
if v.Kind() != reflect.Func {
    return
}
```    

`reflect.Value.Call` 方法是运行时调用方法的入口.   


完整实例: 

```go
package main

import (
	"fmt"
	"reflect"
)

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
	ref := reflect.Indirect(reflect.New(reflect.TypeOf(s)))
	ref.FieldByName(`ID`).SetUint(1)
	ref.FieldByName(`Info`).Set(reflect.ValueOf(userinfo{
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

## 反射的应用
**反射的最佳应用场景是程序的启动阶段, 实现一些类型检查, 注册等前置工作, 既不影响程序性能同时又增加了代码的可读性**.   

### 判断未知对象是否实现了具体接口
通常情况下, 判断未知对象是否实现具体接口很简单, 直接通过 `变量名.(接口名)` 类型验证的方式就可以判断. 但也有例外, 即框架代码实现中检查调用代码的情况. 因为框架代码先实现(并不知道接口名), 调用代码后实现, 也就无法在框架代码中通过简单的类型验证的方式进行验证.   

```go
// 目标接口定义
type Foo interface {
    Bar(int)
}

// 通过对 nil 值的强制转换声明接口的空值
dst := (*Foo)(nil)

// 验证未知变量 src 是否实现了 Foo 接口
dstTypeEl := reflect.TypeOf(dst).Elem()
srcType := reflect.TypeOf(src)
if !srcType.Implements(dstTypeEl) {
    return false
}

```    

### 结构体字段属性标签
### 函数适配


## Tools

* [mitchellh/mapstructure](https://github.com/mitchellh/mapstructure): Go library for decoding generic map values into native Go structures.


## Refs
* [Go 语言设计与实现 - 反射](https://draveness.me/golang/docs/part2-foundation/ch04-basic/golang-reflect/)
* [Go 编程：图解反射](https://www.gitdig.com/go-reflect/)
