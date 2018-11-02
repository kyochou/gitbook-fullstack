# Animation
除了长(`width`), 宽(`height`), 层级(`z-index`)外, 动画为页面带来了第四个维度: 时间.

## Transition
`transition` 的作用在于, 指定状态变化所需要的时间.
它是一个简写的属性, 用于设置四个过渡属性:

```css
transition: property duration timing-function delay;
```

### transition-property
设置过渡效果所作用的 CSS 属性名称.

### transition-duration
设置完成过渡效果需要的时间.

### transition-timing-function
状态变化速度, 即过渡效果的速度曲线. 默认为 ease. 


### transition-delay
定义过渡效果何时开始.
delay 真正意义在于, 它指定了动画发生的顺序, 使得多个不同的 transition 可以连在一起, 形成复杂效果.

## Animation

## Refs
* [CSS 动画简介](http://www.ruanyifeng.com/blog/2014/02/css_transition_and_animation.html)