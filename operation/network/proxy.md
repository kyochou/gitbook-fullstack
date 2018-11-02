# Proxy

## CLI
### FAQs
* 如何在 `sudo` 命令中使用环境变量中的代理设置(HTTP_PROXY, HTTPS_PROXY, FTP_PROXY, etc..)

    关键点是要将当前用户的环境变量代入到 `sudo` 环境中, 可通过 `visudo` 命令将要保留的环境变量添加到 `/etc/sudoers` 文件中:
    
    ```shell
    Defaults        env_keep += "http_proxy https_proxy ftp_proxy"
    ```
    
    参考 ['sudo gem install ...' behind a proxy](http://jacob.stanley.io/2010/10/27/sudo-gem-install-behind-a-proxy/)