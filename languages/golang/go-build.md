# go-build

## `-ldflags`
### `-X`    
* 如果项目中有 `vendor` 目录且没有使用 `gomodule`, 则在指定路径时要相对于 `GOPATH` 指定.     
* 如果要赋值的变量中包含空格, 则需要用引号将 `-X` 后面的变量和值都引起来.   

#### Refs
* [Golang -ldflags 的一个技巧](https://ms2008.github.io/2018/10/08/golang-build-version/)
* [Golang编译-ldflags -X 在vendor中不生效的问题](https://studygolang.com/articles/17778)