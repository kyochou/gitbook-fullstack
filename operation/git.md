# Git
## Design
Git 中的大多数输出, 都会定向于 `stderr` 而不是 `stdout`. 使用 `-q` 选项可以在未出错时屏蔽输出. 但也有例外. 如 `push` 操作中服务器返回的数据. 此数据只能手动处理:
```
```


### Refs
* [git stderr(错误流) 探秘](https://juejin.im/entry/5b96509c5188255c56448677)