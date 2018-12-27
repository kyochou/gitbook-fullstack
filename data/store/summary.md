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

1. 更新代码后, 先执行 `dbmate up` 将迁移文件同步至数据库. 
2. 需要修改数据库结构时, 不要直接修改数据库, 使用 `dbmate new` 命令生成一个新的迁移文件, 在其内添加更新和回滚语句.
3. 执行 `dbmate up` 将迁移文件更新至数据库. 注意, 执行此操作后, 迁移文件就不应该再被修改. 需要再次添加新的数据库语句时, 根据语义来选择操作:
    1. 使用 `dbmate new` 重新创建一个迁移文件.
    2. 使用 `dbmate down` 命令回滚上次的迁移然后再修改此迁移文件.
4. 提交到代码仓库中的迁移文件禁止修改(有可能别人已经执行过了). 
