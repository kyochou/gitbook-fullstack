# software

## custom

* 使应用程序不显示在 Dock, `CMD+Tab` 中
    修改程序的 `Info.plist` 文件, 在 `dict` 下添加:
    
    ```xml
        <key>LSUIElement</key>
        <string>1</string>
    ```
    参考 [Is there a way to hide certain apps from the cmd+tab menu?](https://apple.stackexchange.com/questions/92004/is-there-a-way-to-hide-certain-apps-from-the-cmdtab-menu)
    
    
## window
* [HazeOver](https://hazeover.com/): 为失去焦点的应该添加蒙版以和当前应用区分. Automatically highlights the front window by fading out all the background windows.
* [QSpace](https://apps.apple.com/cn/app/id1469774098): 多视图文件管理(与 TotalFinder 有类似).
* [Magnet](https://magnet.crowdcafe.com/): Organize Your Workspace. 调整窗口大小的分屏软件. 功能与 Spectacle 类似.

### Status Bar
* [Mortennn/Dozer](https://github.com/Mortennn/Dozer): Hide status bar icons on macOS.
    
## network
* [Little Snitch](https://www.obdev.at/products/littlesnitch/index.html): 防火墙.


## fs
* [syncthing/syncthing](https://github.com/syncthing/syncthing): Open Source Continuous File Synchronization.  

### proxy

* 使用 SSH 服务器做为 socks5 代理: `/usr/bin/ssh -N -f -D 19997 kjprod`.
* 使用 polipo 将 socks5 代理转换为 http:
    1. `brew install polipo`.
    2. 创建配置文件 `~/.polipo`:
        socksParentProxy = 127.0.0.1:19997
        socksProxyType = socks5
        proxyAddress = 127.0.0.1
        proxyPort = 19999       

    3. 启动服务: `brew services start polipo`.



### Tools

* 有道词典: 会占用全局的 `cmd-`, `cmd+`, `cmd0` 等快捷键从而导致其他应用的快捷键不生效. 此时重新启动下有道词典即可. 
* Dictionary.app: 
    * 导入第三方词库: 
        * 下载词库: http://download.huzheng.org/zh_CN/.
        * 使用工具 [DictUnifier](https://github.com/jjgod/mac-dictionary-kit) 或 [dictconv](https://github.com/ritou11/dictconv) 将词库转换为 Mac 字典格式. 然后放在词库目录下: `mv <转换完输出的路径> ~/Library/Dictionaries`.
        * 进入 "词典->设置" 勾选新添加的词典.
    * 在 Trackpad 中开启三指轻点查询(look up)功能.
    * 参考: 
        * [把查词做到极致的 macOS 原生词典，其实很好用](https://sspai.com/post/43155)
        * [2021 Macbook Pro 自带词典添加词库](https://zhuanlan.zhihu.com/p/365794977)
