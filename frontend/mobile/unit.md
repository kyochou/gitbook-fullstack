# 像素尺寸单位

## Practice

* 使用 rpx&vw 做为适配方案. rpx 做为主要尺寸单位, vw, wh 辅助使用. 屏幕总宽度为 750rpx, 即: `750rpx = 100vw= 100%`.
* 开发设计都以 iPhoneX(375x812) 为屏幕尺寸进行调试.
* 设计稿设计时以 iPhoneX(375x812) 的屏幕尺寸为像素单位设计, 然后放大 2 倍交付(即设计时使用 375 宽, 出图时使用 750 宽. 不直接使用 750 为基准设计是为了防止出现非整数单位). 图标直接出 @2x, @3x 的图即可(@1x 对应的机型已经被淘汰了). **在 750 的设计稿上, 1px=1rpx**.





## Term

dpr(Device Pixel Ratio) 是显示设备的物理像素(physical pixels)和逻辑像素(px, 设备无关像素, device-independent pixels)的比值. 即显示设备使用几个像素点显示一个逻辑像素.  

rem 是相对长度单位, 指相对于页面根元素 html 的字体的大小. 

rpx(responsive pixel) 是微信小程序解决自适应屏幕尺寸的尺寸单位, 可以根据屏幕宽度进行自适应. 规定屏幕宽为750rpx. 如在 iPhone6 上, 屏幕宽度为375px, 共有 750 个物理像素, 则 750rpx = 375px = 750物理像素, 即 1rpx = 0.5px = 1物理像素.
微信小程序的长度单位 rpx 的换算方式为: `rpx = px * (目标设备宽 px 值 / 750)`. 举个例子:
* 目标设备的宽度如果是 375px(@1x的尺寸), 按照 750rpx 进行换算, 则等于 1rpx = 0.5px
* 目标设备的宽度如果是 750px(@2x的尺寸), 换算后 1rpx = 1px
* 目标设备的宽度如果是 1125px(@3x的尺寸), 换算后 1rpx = 1.5px


## Refs

### rpx
* [推荐一个新的移动端多屏适配方案（h5+小程序）](https://juejin.cn/post/6844903733302673421)

### rem
* [rem, vw, 还是...? 各凭本事的移动端适配方案](https://juejin.im/post/5bc07ebf6fb9a05d026119a9)
* [再聊移动端页面的适配](https://www.w3cplus.com/css/vw-for-layout.html)
* [如何在 Vue 项目中使用 vw 实现移动端适配](https://www.w3cplus.com/mobile/vw-layout-in-vue.html)
* [分享手淘过年项目中采用到的前端技术](https://www.w3cplus.com/css/taobao-2018-year.html)
