# Summary

## Libraries

* [Find The Best Vuejs Resources For Your Project](https://bestofvue.com/)


### Common

* [qs](https://github.com/ljharb/qs): A querystring parser with nesting support.
* [sprintf.js](https://github.com/alexei/sprintf.js): sprintf.js is a complete open source JavaScript sprintf implementation.
* [date-fns](https://date-fns.org/): Modern JavaScript date utility library. support tree-shaking.
* [axios](https://github.com/axios/axios): Promise based HTTP client for the browser and node.js.

### Refs
* 日期库的选择参见 [You-Dont-Need-Momentjs](https://github.com/you-dont-need/You-Dont-Need-Momentjs)

### UI
PC 页面: [bootstrap-vue](https://github.com/bootstrap-vue/bootstrap-vue).
Mobile 页面: [vant](https://github.com/youzan/vant).
中后台: [vue-manage-system](https://github.com/lin-xin/vue-manage-system) based on ElementUI.

## DOM
### Event
#### scroll

```
data () {
  return {
    scrolled: false
  };
},
methods: {
  handleScroll () {
    this.scrolled = window.scrollTop > 0;
  }
},
mounted () {
  window.addEventListener('scroll', this.handleScroll);
},
beforeDestroy () {
  window.removeEventListener('scroll', this.handleScroll);
}
```


## MVVM
data 中定义的值固定的, 定义后就不会自动改变.
计算属性(computed)可以根据其所依赖的响应式变量的变化而重新求值, 但它是基于它们的依赖进行缓存的. 计算属性只有在它的相关依赖发生改变时才会重新求值. 如果不希望有缓存, 请用 method 替代.
更复杂的变化可以考虑 watched 属性.

请反复阅读 Vue 的 [数组更新检测](https://cn.vuejs.org/v2/guide/list.html#%E6%95%B0%E7%BB%84%E6%9B%B4%E6%96%B0%E6%A3%80%E6%B5%8B). 明确了解什么时候应该使用 `this.$set`, `this.$delete` 进行相关操作.

### watch
如果 `watch` 的对象需要在初始化时立即执行一次, 可将其 `immediate` 属性设置为 `true`.

## FAQs
* 动态修改 DOM 属性
    将 DOM 属性绑定到 Vue 的变量上, 修改 Vue 变量值即可.

## Note
仔细想想, 几乎任意类型的应用界面都可以抽象为一个组件树.
在 Vue 里, 一个组件本质上是一个拥有预定义选项的 Vue 实例.
每个 Vue 实例都会代理其 data 对象里所有的属性.
Vue.js 没有 "控制器" 的概念. 组件的自定义逻辑可以分布在钩子中.
指令(Directives)的职责就是当表达式的值改变时相应地将某些行为应用到 DOM 上.
过滤器只可以用在 mustache 插值和 v-bind 表达式中. 因为过滤器设计的目的就是用于文本转换.
模板中放入太多的逻辑会让模板过重且难以维护.
当你想要在响应数据变化时执行异步操作或开销较大的操作, 使用 Watchers 是很有用的.
一般来说, v-if 有更高的切换开销, v-show 有更高的初始渲染开销.
在 Vue 中, 父子组件的关系可以总结为 props down, events up. 父组件通过 props 向下传递数据给子组件, 子组件通过 events 给父组件发送消息.
组件作用域简单地说是: 父组件模板的内容在父组件作用域内编译; 子组件模板的内容在子组件作用域内编译.
不要在 Vue 实例的属性和回调上使用箭头函数, 因为箭头函数的 this 绑定的的是当前的 Context, 并不指向 Vue 实例本身(Vue 的 webpack 插件已经可以自动将箭头函数转换为一般函数了).

### 生命周期
#### Note
对 Dom 的操作放在 `mounted` 方法中进行, 即在 Vue 组件完成 Dom 的挂载后再操作.
可以手动调用 Vue 的 destroy 方法 `this.$destroy()` 将当前组件回收.

## Webpack
### Refs

* [vue 多页面开发和打包的正确姿势](https://juejin.im/post/5a8e3f00f265da4e747fc700)

## Vuex
### 是什么
Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式. 它采用集中式存储管理应用的所有组件的状态, 并以相应的规则保证状态以一种可预测的方式发生变化.

### 基本思想
把组件的共享状态抽取出来, 以一个全局单例模式管理. 在这种模式下, 我们的组件树构成了一个巨大的 "视图", 不管在树的哪个位置, 任何组件都能获取状态或者触发行为.

### 核心概念
#### State
Vuex 使用单一状态树. 用一个对象就包含了全部的应用层级状态. 至此它便作为一个 "唯一数据源(SSOT)" 而存在. 这也意味着, 每个应用将仅仅包含一个 store 实例.
可以使用 `mapState` 辅助函数帮助我们生成计算属性.

#### Getter
Vuex 允许我们在 store 中定义 "getter". 就像计算属性一样, getter 的返回值会根据它的依赖被缓存起来, 且只有当它的依赖值发生了改变才会被重新计算.
可以使用 `mapGetters` 辅助函数帮助我们生成计算属性.

#### Mutation
更改 Vuex 的 store 中的状态的唯一方法是提交 mutation. Vuex 中的 mutation 非常类似于事件: 每个 mutation 都有一个字符串的事件类型(type)和一个回调函数(handler). 这个回调函数就是我们实际进行状态更改的地方.
要唤醒一个 mutation handler, 你需要以相应的 type 调用 store.commit 方法.
使用常量替代 Mutation 事件类型. 这样可以使 linter 之类的工具发挥作用.
Mutation 必须是同步函数. 在 Vuex 中, Mutation 都是同步事务.
可以使用 `mapMutations` 辅助函数帮助我们将组件中的 methods 映射为 `store.commit` 调用.

#### Action
Action 类似于 Mutation, 不同在于: Action 提交的是 Mutation, 而不是直接变更状态; Action 可以包含任意异步操作.
Action 通过 `store.dispatch` 方法触发.
可以使用 `mapActions` 辅助函数帮助我们将组件中的 methods 映射为 `store.dispatch` 调用.

#### Module
Vuex 允许我们将 store 分割成 Module, 每个 Module 拥有自己的 state, getter, mutation, action, 甚至是嵌套子 Module.
默认情况下, 模块内部的 action, mutation 和 getter 是注册在全局命名空间的 --- 这样使得多个模块能够对同一 mutation 或 action 作出响应. 通过将 module 的 namespaced 属性设为 ture 的方式为其添加命名空间.
在 store 创建之后, 你可以通过 `store.registerModule` 的方法注册模块.

### 约定
* 应用层级的状态应该集中到单个 store 对象中.
* 提交 mutation 是更改状态的唯一方法, 并且这个过程是同步的.
* 异步逻辑都应该封装到 action 里面.


## Refs
* [Vue.js](https://vuejs.org/)
* [Vue2 技术栈归纳与精粹](https://uinika.github.io/web/vue.html)
* [vue 技术分享 - 你可能不知道的 7 个秘密](https://www.haorooms.com/post/vue_7secret)
* [Vue 高版本中一些新特性的使用](https://github.com/masterkong/blog/issues/7)