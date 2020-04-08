# 字符串
Go 中 `string` 是内置类型, 同时它与普通的 `slice` 类型有着相似的性质. 例如可以进行切片操作.

* `strings` 包提供了很多操作字符串的简单函数.
* `strconv` 包提供了基本数据类型和字符串之间的转换.
* `regexp` 包提供了正则表达式功能.
* `unicode` 包及其子包 `unicode/utf8` 提供了对 Unicode 相关编码, 解码的支持.

## strings
* 是否包含: `Contains`, `ContainsAny`, `ContainsRune`.
* 出现次数: `Count`.
* 前缀, 后缀: `HasPrefix`, `HasSuffix`.
* 出现位置: `Index`, `IndexAny`, `IndexFunc`, `IndexRune`, `LastIndex`, `LastIndexAny`, `LastIndexFunc`.
* 分割为数组: `Fields`, `FieldsFunc`, `Split`, `SplitAfter`, `SplitN`, `SplitAfterN`.
* 数组合并为字符串: `Join`.
* 重复: `Repeat`.
* 替换: `Replace`, `type Replacer`.
* Reader 类型.

## bytes
该包定义了一些操作 `[]byte` 的便利操作. 其实现的功能与 `strings` 包类似.

## strconv
进制的取值为 `2~36`.
`fmt.Sprintf` 也可实现类型转换, 但性能比较差.

* 字符串转换为整型: `ParseInt`, `ParseUint`, `Atoi`.
* 整型转为字符串: `FormatUint`, `FormatInt`, `Itoa`.
* 布尔值: `ParseBool`, `FormatBool`, `AppdendBool`.
* 浮点数: `ParseFloat`, `FormatFloat`, `AppendFloat`.
* Quote 函数: 返回使用双引号括起来后的字符串.

## regexp

参考 [StefanSchroeder/Golang-Regex-Tutorial](https://github.com/StefanSchroeder/Golang-Regex-Tutorial).

## unicode