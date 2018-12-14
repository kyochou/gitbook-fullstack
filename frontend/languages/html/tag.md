# # Tags

## Meta
### referrer
用于表示从哪儿链接到目前的网页. 借着 HTTP 来源地址, 可以检测访客从哪里而来. 常用于防止图片等资源被盗链.

#### Refs
* [主流浏览器图片反防盗链方法总结](https://blog.mythsman.com/2018/04/20/1/)
    
## 加载资源

### 预加载 
`preload` 的设计初衷是为了让当前页面的关键资源尽早被发现和加载, 从而提升首屏渲染性能.
`prefetch` 是为了提前加载下一个导航所需的资源, 提升下一次导航的首屏渲染性能. 但也可以用来在当前页面提前加载运行过程中所需的资源, 加速响应.

#### 实践
* 使用 `preload` 加载字体可以避免文字闪动问题.


#### Prefetch
`prefetch` 告诉浏览器用户下一步操作可能需要用到的资源. 浏览器识别到 Prefetch 时, 会延迟加载该资源且不执行, 等到真正请求相同资源时, 就能够得到更快的响应.

#### preload 
`preload` 是一个预加载关键字. 它显式地向浏览器声明一个需要提前加载的资源. 可以在 `head` 中的 `link` 标签或 HTTP 头部的 `Link` 字段中声明实现.
使用 Preload 加载资源的方式有以下几个特点:
* 提前加载资源.
* 资源的加载和执行分离.
* 不延迟网页的 `load` 事件(除非 Preload 资源刚好是阻塞 window 加载的资源).

任何你想要先加载后执行, 或者想要提高页面渲染性能的场景都可以使用 Preload.

### DNS Prefetching
DNS prefetching 允许浏览器在用户浏览页面时后台运行 NDS 的解析.

### Prerendering
Prerendering 和 prefetching 非常相似, 它们都优化了可能导航到的下一页上的资源的加载, 区别是 prerendering 在后台渲染了整个页面.

### Preconnect
`preconnect` 允许浏览器在一个 HTTP 请求正式发给服务器前预先执行一些网络操作.

#### Refs
* [资源提示 —— 什么是 Preload，Prefetch 和 Preconnect？](https://juejin.im/post/5b5984b851882561da216311)
* [Preload，Prefetch 和它们在 Chrome 之中的优先级](https://juejin.im/post/58e8acf10ce46300585a7a42)
* [有一种优化，叫 Preload](http://imhxl.com/post/preload.html)
* [Prefetch 和预加载实践](http://imhxl.com/post/prefetch-and-pre-practice.html)

### script
#### 加载和执行

脚本文件在下载时, 浏览器对 Dom 的渲染会被阻止. 因为脚本文件可能会改变 Dom 的结构.
`script` 标签的 `async` 属性告诉浏览器, 此脚本加载的时候, 不需要浏览器停止 Dom 的渲染. 只要此脚本下载完毕, 就开始执行脚本.
`script` 标签的 `defer` 属性与 `async` 类似, 只是执行的时机不是脚本下载完毕, 而是等着整个 Dom 解析完成后执行. 另外, 使用 `defer` 属性的脚本是按顺序执行的.

![async-vs-defer](http://www.growingwiththeweb.com/images/2014/02/26/async-vs-defer-twitter.png)

参考: [不要再把 script 标签放 body 末尾了](
https://www.chrisyue.com/please-dont-put-script-tag-at-the-end-of-body.html)
