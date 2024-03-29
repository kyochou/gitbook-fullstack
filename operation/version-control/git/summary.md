# Git
## Practices
* Git 中的大多数输出, 都会定向于 `stderr` 而不是 `stdout`. 使用 `-q` 选项可以在未出错时屏蔽输出. 但也有例外. 如 `push` 操作中服务器返回的数据. 此数据只能手动处理:

    ```shell
    $ usr/bin/git push -q &>/dev/stdout | grep -vi 'remote:'
    ```

* git 对二进制文件也具备相当不错的可用性, 只是需要配套的 diff 工具. 参考: 
[gitattributes](https://git-scm.com/docs/gitattributes), [git-diff-image](https://github.com/ewanmellor/git-diff-image).

### Refs
* [git stderr(错误流) 探秘](https://juejin.im/entry/5b96509c5188255c56448677)




## 设置

### 账号

```shell

# 将访问方式由 https 协议转换为 git 协议
git config --global url."git@github.com:".insteadOf "https://github.com/"

```

#### Refs
* [解决 go get (github) private repos 权限问题](https://www.gitdig.com/post/go-get-private-github-repo/)

### 属性
在 `.gitattributes` 文件中可设置 git 的相关属性.
可以用 Git 属性让 Git 知道哪些是二进制文件(以防它没有识别出来), 并指示其如何处理这些文件. 例如, 一些文本文件是由机器产生的, 没办法进行比较(比如 XCode .pbxproj 文件), 但一些二进制文件是可以进行比较的(如果图片的属性). 
使用 `docx2txt` 比较 Word 文件:
```shell
echo '*.docx diff=word' >> .gitattributes
git config diff.word.textconv docx2txt
```

使用 `exiftool` 比较图像的 EXIF 信息:
```shell
echo '*.png diff=exif' >> .gitattributes
git config diff.exif.textconv exiftool
```

## Github
### FAQs
* [如何同步 Github fork 出来的分支](https://jinlong.github.io/2015/10/12/syncing-a-fork/)
* [签名你的每个 Git Commit](https://sexywp.com/sign-your-every-git-commit.htm)

## Tools
* [toptal/gitignore.io](https://github.com/toptal/gitignore.io): Create useful .gitignore files for your project.
* [jonas/tig](https://github.com/jonas/tig): Text-mode interface for git. 可用来方便的查看 log.
* [git-time-metric/gtm](https://github.com/git-time-metric/gtm): Simple, seamless, lightweight time tracking for Git.  
* [augmentable-dev/gitqlite](https://github.com/augmentable-dev/gitqlite): gitqlite is a tool for running SQL queries on git repositories.    
* [sobolevn/git-secret](https://github.com/sobolevn/git-secret): A bash-tool to store your private data inside a git repository.

