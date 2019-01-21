# FileSystem

## 文本文件
### FAQs
* 在计算领域中, Shebang(也称为 Hashbang) 是一个由井号和叹号构成的字符序列 `#!`, 其出现在文本文件的第一行的前两个字符. 在文件中存在 Shebang 的情况下, 类 Unix 操作系统的程序加载器会分析 Shebang 后的内容, 将这些内容作为解释器指令, 并调用该指令, 然后将载有 Shebang 的文件路径作为该解释器的参数.

    常见的 Shebang 解释器写法有两种, 直接指定解释器路径(`#!/bin/bash`)或自 ENV 中读取(`#!/usr/bin/env bash`).