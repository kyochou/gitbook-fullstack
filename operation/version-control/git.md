# Git
## Practices
* Git 中的大多数输出, 都会定向于 `stderr` 而不是 `stdout`. 使用 `-q` 选项可以在未出错时屏蔽输出. 但也有例外. 如 `push` 操作中服务器返回的数据. 此数据只能手动处理:

    ```shell
    $ usr/bin/git push -q &>/dev/stdout | grep -vi 'remote:'
    ```

### Refs
* [git stderr(错误流) 探秘](https://juejin.im/entry/5b96509c5188255c56448677)


## 设置
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


## Tools
* [toptal/gitignore.io](https://github.com/toptal/gitignore.io): Create useful .gitignore files for your project.
* [jonas/tig](https://github.com/jonas/tig): Text-mode interface for git. 可用来方便的查看 log.