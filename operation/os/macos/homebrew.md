# Homebrew


## Services
服务配置:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
  <dict>
    <key>Label</key>
    <string>服务名称:homebrew.mxcl.haproxy</string>
    <key>KeepAlive</key>
    <true/>
    <key>ProgramArguments</key>
    <array>
      <string>服务路径:/usr/local/opt/haproxy/bin/haproxy</string>
      <string>服务参数:-f</string>
      <string>服务参数:/usr/local/etc/haproxy.cfg</string>
    </array>
    <key>StandardErrorPath</key>
    <string>错误日志路径:/usr/local/var/log/haproxy.log</string>
    <key>StandardOutPath</key>
    <string>日志路径:/usr/local/var/log/haproxy.log</string>
  </dict>
</plist>
```

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