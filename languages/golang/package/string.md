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
* 