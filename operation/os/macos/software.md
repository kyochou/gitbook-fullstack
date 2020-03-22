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
    
## network
* [Little Snitch](https://www.obdev.at/products/littlesnitch/index.html): 防火墙.


## fs
* [syncthing/syncthing](https://github.com/syncthing/syncthing): Open Source Continuous File Synchronization.  

### proxy

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



### Tools
* 有道词典: 会占用全局的 `cmd-`, `cmd+`, `cmd0` 等快捷键从而导致其他应用的快捷键不生效. 此时重新启动下有道词典即可.