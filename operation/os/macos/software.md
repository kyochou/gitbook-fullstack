# Software

## Homebrew
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
    
## proxy

* 使用 SSH 服务器做为 socks5 代理: `/usr/bin/ssh -N -f -D 19997 kjprod`.
* 使用 polipo 将 socks5 代理转换为 http:
    1. `brew install polipo`.
    2. 创建配置文件 `~/.polipo`:

        ```ini
        socksParentProxy = 127.0.0.1:19997
        socksProxyType = socks5
        proxyAddress = 127.0.0.1
        proxyPort = 19999   
        ```
    
    3. 启动服务: `brew services start polipo`.
    