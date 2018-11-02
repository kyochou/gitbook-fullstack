# iOS

## CocoaPods
在 install, update 时添加 no-repo-update 参数可以显著提高速度. 原因是 CocoaPods 在执行 install, update 命令的时候会升级本地的 spec 仓库, 这个参数可以省略这一步：
```cli
pod install --verbose --no-repo-update
pod update --verbose --no-repo-update
```
### Refs
* [CocoaPods pod install/pod update更新慢的问题](http://www.cnblogs.com/yiqiedejuanlian/p/3698788.html)

## XCode
### Practices
* 账号导出(供不同机器之间共享)[Exporting Your Signing Identities and Provisioning Profiles](https://developer.apple.com/library/ios/recipes/xcode_help-accounts_preferences/articles/export_signing_assets.html)
* XCode7 版本的模拟器路径位于 ~/Library/Developer/CoreSimulator/Devices 目录下. 可以通过以下方法得到程序所在的路径:

    ```objc
      NSString *homeDirectory = NSHomeDirectory();
      NSLog(@"path:%@", homeDirectory);
    ```  

### 更新证书后常见的问题
* [XCode 6 出现 no identity found: Command /usr/bin/codesign failed with exit code 1 解决方法汇总](http://www.cnblogs.com/duwei/p/4272075.html)
* [更新证书错误：No matching provisioning profiles found](http://www.cnblogs.com/flycantus/p/3492071.html)