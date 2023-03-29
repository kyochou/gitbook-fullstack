# Layout

## Practices

### 解决方案
* 使用 rpx 做为像素单位.

### Note
更少层次的嵌套构建出的网页更优雅.



## Term

dpr(Device Pixel Ratio) 是显示设备的物理像素(physical pixels)和逻辑像素(px, 设备无关像素, device-independent pixels)的比值. 即显示设备使用几个像素点显示一个逻辑像素.  

微信小程序的长度单位 rpx 的换算方式为: `rpx = px * (目标设备宽 px 值 / 750)`. 举个例子:
* 目标设备的宽度如果是 375px(@1x的尺寸), 按照 750rpx 进行换算, 则等于 1rpx = 0.5px
* 目标设备的宽度如果是 750px(@2x的尺寸), 换算后 1rpx = 1px 
* 目标设备的宽度如果是 1125px(@3x的尺寸), 换算后 1rpx = 1.5px


## Design

开发时可将模拟设备的尺寸设置为 375X750(iphone6 的尺寸为 375x667, 但现在大多数手机长度都比 667 大, 所以使用 750), Dpr 设置为 2. 
设计以 @1x 为标准(375x750)切图, 切图时直接出 @2x, @3x 的图即可(@1x 对应的机型已经被淘汰了).   

## 自适应布局
### 移动端

使用 rem&vw 适配方案.
#### 参考
* [rem, vw, 还是...? 各凭本事的移动端适配方案](https://juejin.im/post/5bc07ebf6fb9a05d026119a9)
* [再聊移动端页面的适配](https://www.w3cplus.com/css/vw-for-layout.html)
* [如何在 Vue 项目中使用 vw 实现移动端适配](https://www.w3cplus.com/mobile/vw-layout-in-vue.html)
* [分享手淘过年项目中采用到的前端技术](https://www.w3cplus.com/css/taobao-2018-year.html)

## Box Alignment

盒子对齐模块. Flex 和 Grid 使用的都是这一对齐模块.

参考:

* [Web 布局新系统：CSS Grid,Flexbox 和 Box Alignment](https://www.w3cplus.com/css/css-grids-flexbox-and-box-alignment-our-new-system-for-web-layout.html)


## Grid

网格布局的基本概念: 

![网格布局的基本概念](https://user-gold-cdn.xitu.io/2017/11/29/160066e029b70844)

## Flex
布局的传统解决方案, 基于盒状模型, 依赖 `display`, `position`, `float` 属性. 它对于那些特殊布局非常不方便. 比如, 垂直居中就不容易实现.
Flex 布局可以简便, 完整, 响应式地实现各种页面布局.
Flex 是 Flexible Box 的缩写, 意为 "弹性布局", 用来为盒状模型提供最大的灵活性.
设为 Flex 布局后, 子元素的 `float`, `clear`, `vertical-align` 属性将失效.
Flex 容器(Container)默认存在两根轴: 水平的主轴(main axis)和垂直的交叉轴(cross axis).
容器元素(Item)默认沿主轴排列.
Flex 布局的 Item 之间默认没有间隔.

### 属性
`justify-` 适用于主轴, `align-` 适用于交叉轴.

* `flex-direction`: 定义元素的排列方向. 值可以为:
  *  `row`: 默认值.
  *  `row-reverse`
  *  `column`
  *  `column-reverse`
* `flex-wrap`: 定义如果一条轴线排不下, 如何换行. 值可以为:
  * `nowrap`: 默认值, 不换行.
  * `wrap`: 换行, 第一行在上方.
  * `wrap-reverse`: 换行, 第一行在下方.
* `flex-flow`: 为 `flex-direction` 和 `flex-wrap` 的简写. 默认为 `row nowrap`.
* `justify-content`: 定义在主轴上的对齐方式. 值可以为:
  * `flex-start`: 默认值, 开始对齐.
  * `flex-end`: 结束对齐.
  * `center`: 居中对齐.
  * `space-between`: 两端对齐, 元素之间的间隔都相等.
  * `space-around`: 每个元素之间的间隔相等, 元素与边框之间的间隔为元素之间的一半.
* `align-items`: 定义元素在交叉轴上如何对齐. 值可以为:
  * `flex-start`
  * `flex-end`
  * `center`
  * `baseline`: 元素的第一行文字的基线对齐. 
  * `stretch`: 默认值, 如果元素未设置高度或高度设为 `auto`, 将占满整个容器的高度.
* `place-content`: 可以同时设置 `justify-content` 和 `align-centent` 的值.
* `align-centent`: 定义多根轴线的对齐方式. 如果元素只有一根轴线, 该属性不起作用. 值可以为:
  * flex-start
  * flex-end
  * center
  * space-between: 与轴两端对齐, 轴线之间的间隔平均分布.
  * space-around: 每根轴线之间的间隔都相等. 轴线与边框之间的间隔为轴线之间的一半.
  * stretch: 默认值, 轴线点满整个轴.
  
### 元素的属性
* `order`: 定义元素的排列顺序. 越小越靠前. 默认为 0.
* `flex-grow`: 定义元素的放大比例, 默认为 0. 表示即使存在剩余空间, 也不放大.
* `flex-shrink`: 定义元素的缩小比例, 默认为 1. 表示如果空间不足, 元素将缩小.
* `flex-basis`: 定义了在分配多余空间之前, 项目占据的主轴空间. 浏览器根据这个属性, 计算主轴是否有多余空间. 默认为 `auto`, 即项目本来大小. 它可以设置和 `width`, `height` 一样的值.
* `flex`: 为 `flex-grow`, `flex-shrink`, `flex-basis` 的简写. 该属性有两个快捷值: `auto`(1 1 auto)和 `none`(0 0 auto).
* `align-self`: Flex 布局默认不改变 Item 的宽度, 但会改变 Item 的高度. 如果 Item 没有显式指定高度, 就将占据容器的所有高度. `align-self` 属性可以改变此行为. 此属性允许单个元素有与其他元素不一样的对齐方式. 可覆盖容器的 `align-items` 属性. 默认为 `auto`. 表示继承容器的 `align-items` 属性.

### Faqs
* 父容器 `display: flex` 后, 子元素的内部元素 `height: 100%` 无效

    解决办法是父类容器设置 `position: relative`; 子元素设置 `position: absolute; width: 100%; height:100%;`.

### 参考
* [Flex 布局教程：语法篇](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)
* [Flex 布局教程：实例篇](http://www.ruanyifeng.com/blog/2015/07/flex-examples.html)

