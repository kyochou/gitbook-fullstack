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
* 允许重写抽象方法
* 多组命名空间支持尾部逗号: `use Foo\Bar\{Foo,Bar,}`

### Refs
* [PHP7.2](http://www.webzhishi.com/php/php72-new-func.html)
* [PHP 7.2 新功能介绍](https://laravel-china.org/topics/9814/introduction-of-new-functions-of-php-72)
* [从 PHP 7.1.x 移植到 PHP 7.2.x](http://php.net/manual/zh/migration72.php)

## 7.0
* 组合比较符 `<=>`
* `isset` 合并运算符:

    ```php
    $b = $a ?? $c;
    // equals isset
    $b = isset($a) ? $a : $c;
    
    $b = $a ?: $c;
    // equals  !empty
    $b = $a ? $a : $c;
    ```

* 命名空间按组导入: `use Foo\Bar{Foo,Bar}`