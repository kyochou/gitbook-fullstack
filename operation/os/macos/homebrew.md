# Homebrew

## FAQs
* upgrade

    ```shell
    brew update
    # 不要随意执行 upgrade, 会将所有软件更新到最新版本
    # brew upgrade
    brew cleanup
    brew prune
    brew style # check&fix
    ``` 
    
* 只安装不检查以提高速度: `HOMEBREW_NO_INSTALL_CLEANUP=1 HOMEBREW_NO_AUTO_UPDATE=1 brew install`.    
    

## Refs
* [解决 Homebrew 安装软件下载失败](https://shockerli.net/post/homebrew-install-download-error/)