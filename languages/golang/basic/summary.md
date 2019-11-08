# 语法基础

## Basic
Go 语言原生支持 `Unicode` 标准, 所以你可以用 Go 语言处理世界上的任何自然语言.
Go 语言是不需要分号做为语句的结束的, 除非要在一行中将多个语句或声明隔开. 在编译时编译器会自动补全相应的行结束符(就像 javascript).
Go 语言中没有深拷贝. 所有值都是浅拷贝传递. 引用类型会拷贝指针地址.

## 注释
`type`, `const`, `func` 以变量名称为注释的开头, `package` 以 `Package name` 为注释的开头.

### Refs
* [GoDoc的使用](https://www.jianshu.com/p/b91c4400d4b2)


## Package

```go
import `lib/math` // 使用 math 前缀调用包中的公开元素. 如 `math.Sin`.
import . `lib/math` // 可以不使用包名而直接调用包中的公开元素. 如 `Sin`.
import _ `lib/math` // 不能调用包中定义的元素, 但会执行包的 `init` 方法.

```

## Error

### Refs
* [Go 1.13中的错误处理](https://tonybai.com/2019/10/18/errors-handling-in-go-1-13/)