# 图标

基于 svg 的图标解决方案是大势所趋. 但还存在兼容性问题. 所以: 
* 移动端使用 uniapp 的 [icon](https://uniapp.dcloud.net.cn/component/icon.html), [uni-icons](https://uniapp.dcloud.net.cn/component/uniui/uni-icons.html) 组件. 对于自定义图标可通过字体图标实现.
* web 端可使用封装好的 svg 组件. 具体可参考 [element-plus-icons](https://element-plus.org/zh-CN/component/icon.html).


icon 的三种实现方式:
* 传统图片. 即 `png`, `jpg` 等格式的图片. 图片需要考虑不同尺寸以兼容不同设备, 图片的颜色也不好更改;
* 字体图标. 需要引入字体文件. 可以很方便的修改颜色, 但只能是单色;
* svg 图标. 可以分别修改图标中不同部位的颜色, 即支持多色. 但在不同平台下兼容性不一.


## Refs
* [网站图标开发指南](https://mp.weixin.qq.com/s?__biz=MzIxNTQ2NDExNA==&mid=100001059&idx=1&sn=81582acee8afa21189262f4058568b2c)