# Store

## CSV
CSV 全称 Comma Separated Values 或 Character Separated Values.
使用逗号做分隔符的一个好处是可以用 Excel 直接打开.
好的实践是:
* 字段全部使用双引号括起来(`"field"`).
* 如果字段里有双引号则在其前面添加一个额外的双引号进行转义(`"value1""value2"`).


## Database
### Migration
#### dbmate
[dbmate](https://github.com/amacneil/dbmate): A lightweight, framework-agnostic database migration tool.