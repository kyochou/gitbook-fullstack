# 特性

## 7.3
* 函数调用的最后一个参数允许尾随逗号
* 新增 `array_key_first` 和 `array_key_last` 函数
* 新增 `is_countable` 函数
* 允许在 JSON 解析失败时抛出错误(`JSON_THROW_ON_ERROR`)
* 允许 Nowdoc 和 Heredoc 进行缩进
* 正则匹配换成 PCRE2
* `list()` 支持引用传值: `[$a, &$b] = $arr;`

### Refs
* [forecho/php7.3](https://github.com/forecho/php7.3)
* [一篇文章帮你了解 PHP 7.3 更新](https://laravel-china.org/topics/21549)
* [从 PHP 7.2.x 移植到 PHP 7.3.x](http://php.net/manual/zh/migration73.php)

## 7.2
* 参数和返回值的类型声明允许使用 `object` 做为对象类型的统一表达
* 