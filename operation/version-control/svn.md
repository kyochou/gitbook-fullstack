# SVN

## FAQs
* 设置代理. 编辑文件 `~/.subversion/servers`:

    ```ini
    [global]
    http-proxy-host = 127.0.0.1
    http-proxy-port = 19999
    ```
    
* 设置 ignore:

    svn 通过属性来判断如何处理仓库中的文件. 你可以使用 `svn propset` 来设置 `svn:ignore` 的值, 可以是文件名或表达式.
    可以对每个目录单独设置 ignore 属性. 也可以使用 `-R` 参数使指定目录和其子目录都应用设置的属性. 使用 `-F` 参数可以将 ignore 属性的目标放在一个文件. 
    
    ```shell
    svn propset svn:ignore -F .svnignore .
    ```
    
    在使用 `add` 命令时, 不要使用 `*`, 这样会把忽略的文件也添加进仓库. 应使用 `svn add --force .` 命令.
    参考: [使用 svn 进行文件和文件夹的忽略](https://www.jianshu.com/p/c02d8b335495)