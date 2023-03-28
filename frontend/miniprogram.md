# 小程序

dpr(Device Pixel Ratio) 是显示设备的物理像素(physical pixels)和逻辑像素(设备无关像素, device-independent pixels)的比值. 即显示设备使用几个像素点显示一个逻辑像素.  

微信小程序的长度单位 rpx 的换算方式为: `rpx = px * (目标设备宽 px 值 / 750)`. 举个例子:
* 目标设备的宽度如果是 375px(@1x的尺寸), 按照 750rpx 进行换算, 则等于 1rpx = 0.5px
* 目标设备的宽度如果是 750px(@2x的尺寸), 换算后 1rpx = 1px
* 目标设备的宽度如果是 1125px(@3x的尺寸), 换算后 1rpx = 1.5px

开发时可将模拟设备的尺寸设置为 375X750(iphone6 的尺寸为 375x667, 但现在大多数手机长度都比 667 大, 所以使用 750), Dpr 设置为 2. 设计以 @2x 为标准(750x1500)切图(@1x 对应的机型已经被淘汰了)即可.   



## Frameworks
* [didi/mpx](https://github.com/didi/mpx): Mpx是一款致力于提高小程序开发体验的增强型小程序框架.