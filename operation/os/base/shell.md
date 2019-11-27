# Shell
## CLI
### wildcard
Shell 提供了一套完整的字符串模式匹配规则, 称之为元字符. 包括:

* `*`: 匹配 0 或多个字符.
* `?`: 匹配任意一个字符.
* `[list]`: 匹配 list 中的任意单一字符.
* `[!list]`: 匹配除 list 中的任意单一字符.
* `[c1-c2]`: 匹配 c1-c2 中的任意单一字符, 如: `[0-9]`, `[a-z]`.
* `{string1,string2,...}`: 匹配 string1 或 string2 (或更多)其中一个字符串.

### metacharacter
* `cmd1;cmd2`: 顺序执行命令.
* `cmd1&&cmd2`: 当 cmd1 返回真时才执行 cmd2.
* `cmd1||cmd2`: 当 cmd1 返回假时才执行 cmd2.
* `(cmd1;cmd2...)`: 将其内的命令置于当前 shell 中执行, 或用于运算或命令替换.
* `{cmd1;cmd2...}`: 将其内的命令置于子 shell 中执行, 或用于变量替换的界定范围.

### 内置变量


### 转义
* `''`: 硬转义, 其内部所有 shell 元字符, 通配符都会被关掉.
* `""`: 软转义, 其内部只允许出现特定的 shell 元字符: `$` 用于参数替换, ` 用于命令代替.
* `\`: 转义, 去除其后紧跟的元字符或通配符的特殊意义.


### Refs
* [Linux Shell 通配符、元字符、转义符使用实例介绍](https://www.cnblogs.com/chengmo/archive/2010/10/17/1853344.html)

### FAQs
* 将多条 Shell 命令合并执行:
    * 在当前 shell 中执行: `(cmd1,cmd2,cmd3...)`.
    * 在子 shell 中执行: `{cmd1,cmd2,cmd3...}` 或使用 `sh -c` 命令.

* 运行子 Shell:
    * `exec`: 重新启动一个新的 shell(而不是子 shell) 来运行命令.

## Resources
* [打造高效的工作环境 – SHELL 篇](https://coolshell.cn/articles/19219.html)


## Man
### Tools
* [chubin/cheat.sh](https://github.com/chubin/cheat.sh)
* [tldr-pages/tldr](https://github.com/tldr-pages/tldr)
* [isacikgoz/tldr](https://github.com/isacikgoz/tldr): fast tldr.
* [jaywcjlove/linux-command](https://github.com/jaywcjlove/linux-command): 中文.