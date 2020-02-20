# Git

## FQAs
* 设置保存用户信息

    ```shell
    git config --global credential.helper store
    ```
    [How to save username and password in git?](https://stackoverflow.com/questions/35942754/how-to-save-username-and-password-in-git)



## Tools
### CLI
* [o2sh/onefetch](https://github.com/o2sh/onefetch): Git repository summary on your terminal.
* [arzzen/git-quick-stats](https://github.com/arzzen/git-quick-stats): Git quick statistics is a simple and efficient way to access various statistics in git repository. 


## GitHub

### Tools
* [gissue/gissue.github.io](https://github.com/gissue/gissue.github.io/): For downloading GitHub issues as markdown in ZIP file.


## Gitlab

### Git Server-Side Hooks

在 Gitlab 的仓库中(`data/git-data/repositories/`)添加相关钩子.
如一个控制提交钩子的路径为: `data/git-data/repositories/tinygame/php.git/custom_hooks/update.d`.

#### 参考
* [Custom Git Hooks](https://docs.gitlab.com/ee/administration/custom_hooks.html)
* [Customizing Git - Git Hooks](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks)
