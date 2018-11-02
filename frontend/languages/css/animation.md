# Animation
除了长(`width`), 宽(`height`), 层级(`z-index`)外, 动画为页面带来了第四个维度: 时间.

## Transition
`transition` 的作用在于, 指定状态变化所需要的时间.
它是一个简写的属性, 用于设置四个过渡属性:

```css
transition: property duration timing-function delay;
```

需要注意的是, 不是所有 CSS 属性都支持 transition. transition 需要明确知道开始状态和结束状态的具体数值, 才能计算出中间状态.

### transition-property
设置过渡效果所作用的 CSS 属性名称.

### transition-duration
设置完成过渡效果需要的时间.

### transition-timing-function
状态变化速度, 即过渡效果的速度曲线. 默认为 ease. 其值包括:
* linear: 匀速;
* ease: 慢速开始, 然后变快, 然后慢速结束. 相当于 `cubic-bezier(0.25,0.1,0.25,1)`;
* ease-in-out: 慢速开始, 然后变快, 然后慢速结束. 相当于 `cubic-bezier(0.42,0,0.58,1)`;
* ease-in: 加速;
* ease-out: 减速;
* cubic-bezier 函数: 自定义速度模式.

### transition-delay
定义过渡效果何时开始.
delay 真正意义在于, 它指定了动画发生的顺序, 使得多个不同的 transition 可以连在一起, 形成复杂效果.

### 局限
* 需要事件触发, 所以没法在网页加载时自动发生.
* transition 是一次性的, 不能重复发生, 除非一再触发.
* 一条 transition 规则, 只能定义一个属性的变化.

## Animation
用于解决 transition 的一些局限.

## Refs
* [CSS 动画简介](http://www.ruanyifeng.com/blog/2014/02/css_transition_and_animation.html)