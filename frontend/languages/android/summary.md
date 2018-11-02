# Android


## AndroidStudio
* 整合 Genymotion: [Android Studio 1.0.1 + Genymotion安卓模拟器打造高效安卓开发环境](http://www.cnblogs.com/yunfeifei/p/4163347.html)
* 目录结构: [Android Studio目录结构浅析](http://segmentfault.com/a/1190000002963895)
* 配置: [第一次使用Android Studio时你应该知道的一切配置](http://www.cnblogs.com/smyhvae/p/4390905.html)
* 代理配置: [常用开源镜像站整理](http://zengrong.net/post/2374.htm)

### Library
* 多项目共享Library.
    
    ```gradle
    # settings.gradle
    include ':../Demo/library'
    # 为引用的模块指定别名, 注意别名要(全局)唯一, 因为其会在引用的模块下生成 <projectname>.iml 文件
    project(":../Demo/library").name = "library4gtd"
    ```

#### Refs
* [Android Studio 删除 Module](http://www.cnblogs.com/xingfuzzhd/p/3433530.html)
* [Android Studio中多项目共享Library](http://www.aswifter.com/2015/06/20/android-studio-share-library/)


## Libraries
  [androidannotations](https://github.com/excilys/androidannotations)

## Practices
  [Android 开发中，有哪些坑需要注意？](http://zhuanlan.zhihu.com/zmywly8866/20309921)