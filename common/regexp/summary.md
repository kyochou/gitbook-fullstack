# RegExp

常用正则表达式规则有两套, 一套是由 IEEE 制定的 POSIX Extended 正则(`EREG`, Extended Regular Expressions), 一套是基于 Perl 语言的 `PREG`(Perl Regular Expressions).
`PCRE`(Perl Compatible Regular Expression)做为 `PREG` 的实现被广泛使用.

## PCRE
* `贪婪匹配(greedy)` 与 `非贪婪匹配(lazy)`: 正则表达式一般趋向于最大长度匹配, 也就是所谓的贪婪匹配. 在量词后加 `?` 可实现非贪婪匹配效果, 非贪婪匹配会优先匹配 `?` 后面的表达式, 然后再回来匹配当前表达式. 非贪婪匹配一般通过回溯实现, 使用时需注意性能问题.