# Component
## 配置项
* inheritAttrs: 我们都知道假如使用子组件时传了 a,b,c 三个 prop, 而子组件的 props 选项只声明了 a 和 b, 那么渲染后 c 将作为 html 自定义属性显示在子组件的根元素上. 如果不希望这样, 可以设置子组件的配置项 `inheritAttrs:false`, 根元素就会干净多了.
* provide/inject: 如果列举 Vue 组件之间的通信方法, 一般都会说通过 prop, 自定义事件, 事件总线, 还有 Vuex. `provide/inject` 提供了另一种方法.

    这对选项需要一起使用, 以允许一个祖先组件向其所有子孙后代注入一个依赖, 不论组件层次有多深, 并在起上下游关系成立的时间里始终生效. 这与 React 的上下文特性(context)很相似.
    provide 和 inject 主要为高阶插件/组件库提供用例, 并不推荐直接用于应用程序代码中.
    

## 实例属性
* \$attrs: 包含了父作用域中不作为 prop 被识别(且获取)的特定绑定(class 和 style 除外). 当一个组件没有声明任何 prop 时, 这里会包含所有父作用域的绑定(class 和 style 除外), 并且可以通过 `v-bind="$attrs"` 传入内部组件 -- 在创建高级别的组件(父子孙多级别的组件)时非常有用.
* \$listeners: 包含了父作用域中的(不含 `.native` 修饰器的) v-on 事件监听器. 它可以通过 `v-on="$listeners" 传入内部组件 -- 在创建更高层次的组件时非常有用.

## 内置组件
### keep-alive
#### 动态缓存
* 使用 `<keep-alive>` 组件及其 `include`, `exclude` 属性动态实现组件的缓存:

    将其 `include` 或 `exclude` 属性绑定到变量, 动态改变变量值即可.

* 使用 `<keep-alive>` 组件结合 `vue-router` 组件动态实现组件的缓存:

    ```
    <!-- v-keep-scroll-position 用来保存滚动条的位置, 除了在 router-view 组件上添加此属性外, 还需要在实际滚动的元素上添加此属性 -->
    <keep-alive>
        <router-view v-keep-scroll-position v-if="$route.meta.keepAlive"></router-view>
    </keep-alive>
    <router-view v-if="!$route.meta.keepAlive"></router-view>
    ```
    Refs: 
        [希望 keep-alive 能增加可以动态删除已缓存组件的功能](https://github.com/vuejs/vue/issues/6509)
        [vue-keep-scroll-position](https://github.com/beeplin/vue-keep-scroll-position)
        [vue-router 之 keep-alive](https://www.jianshu.com/p/0b0222954483)
        [vue 实现前进刷新，后退不刷新](https://juejin.im/post/5a69894a518825733b0f12f2)
        [使用 KEEPALIVE 对上下拉刷新列表数据 和 滚动位置细节处理 – VUE](http://sparkgis.com/java/2018/02/%E4%BD%BF%E7%94%A8keepalive%E5%AF%B9%E4%B8%8A%E4%B8%8B%E6%8B%89%E5%88%B7%E6%96%B0%E5%88%97%E8%A1%A8%E6%95%B0%E6%8D%AE-%E5%92%8C-%E6%BB%9A%E5%8A%A8%E4%BD%8D%E7%BD%AE%E7%BB%86%E8%8A%82%E5%A4%84%E7%90%86/)
       
#### 销毁缓存 
* 缓存组件的父组件被销毁时, 缓存也会被销毁. 在需要销毁使用 `<keep-alive>` 缓存的多个组件时优先考虑是否能直接销毁其父组件.
* 防止组件被缓存:
    
    ```js
    deactivated() {
      this.$destroy()
    },
    ```

    
    
## 组件交互
### 所有组件
定义一个单独的 Vue 对象只用来做事件交互, 从而实现全局事件总线的效果. 参考 [使用 Vue.js 创建全局事件总线（Global Event Bus ）](http://www.pilishen.com/posts/Creating-a-Global-Event-Bus-with-VueJs)

### 父组件到子组件
子组件初始化时, 父组件可以通过属性的形式(`prop`)将值传递给子组件.
子组件运行时, 父组件通过为子组件设置的 `ref` 属性获取子组件对象, 然后通过子组件的 `$emit` 方法调用子组件使用 `$on` 方法定义的事件.
```js
// 父组件中
this.$refs['list'].$emit('reload')
// 子组件中
this.$on('reload', () => { this.reload() })
```

### 子组件到父组件
子组件在运行时通过 `$emit` 方法调用父组件监听的事件.
```js
// 父组件中
<list ref="list" @load-data="loadData">
// 子组件中
this.$emit('load-data')
```

如果你只在子组件里改变父组件的一个值, 可以使用 `$emit('input')`, 会直接改变 `v-model` 的值.

### Refs
* [vue 组件通信 -- 注意事项及经验总结](https://juejin.im/post/5bc092806fb9a05cf2301f25)
* [Vue.js 父子组件通信的十种方式](https://juejin.im/post/5bd18c72e51d455e3f6e4334)


## 无渲染组件
无渲染组件是一个不需要渲染任何自己的 HTML 的组件. 相反, 它只管理状态和行为. 它会暴露一个单独的作用域, 让父组件或消费者完全控制应该渲染的内容.
一个无渲染组件暴露一个单一的作用域插槽, 使用者可以提供他们想要渲染的整个模板.
将一个组件分解成一个表示组件(Presentational Component)和一个无渲染组件(Renderless Component)是一种非常有用的模式, 可以使组件重用变得更加容易.
如果一个组件看起来和它使用的地方是一样的或者只需要一个组件, 那么就没有必要这样做, 因为将所有东西都保存在一个组件中, 会让你的事情变得简单的多.

参考: 

  *  [Vue 中的无渲染组件](https://www.w3cplus.com/vue/renderless-components-in-vuejs.html)
  *  [Renderless Components in Vue.js](https://adamwathan.me/renderless-components-in-vuejs/)

## 异步组件
## 动态导入
动态导入允许程序在运行时加载模块.

```javascript
// 静态导入
import utils from './utils'
// 动态导入
if (<condition>) {
    import ('./utils').then(utils => {
        // use utils
    })
}
```

## 延迟加载 
延迟加载对于应用程序路由有很大的意义, 并且有很大的影响, 因为每个路由都是应用程序的不同部分.
延迟加载可以使组件延迟渲染.

```javascript
<template>
    <div>
        <Tooltip  v-if="show"/>
    </div>
</template>

<script>
    export default {
        data: () => ({
            show: false
        }),
    components: {
        Tooltip: () => import('./components/Tooltip')
    }
}
```

延迟加载的其他参数:

```javascript
const Tooltip = () => ({
    component: import('./components/Tooltip'),
    loading: AwesomeSpinner, // 加载组件
    delay: 200, // 延迟时间
    error: SadFaceComponent, // 错误组件
    timeout: 0, // 加载超时时间
})
```

### Refs
* [Vue 中的异步组件](https://www.w3cplus.com/vue/async-vuejs-components.html)

## 共享
* [详解：Vue cli3 库模式搭建组件库并发布到 npm](https://juejin.im/post/5bbab9de5188255c8c0cb0e3)

## Refs
* [探索 Vue 高阶组件](http://hcysun.me/2018/01/05/%E6%8E%A2%E7%B4%A2Vue%E9%AB%98%E9%98%B6%E7%BB%84%E4%BB%B6/)
* [Vue.js 实用技巧（二）](https://zhuanlan.zhihu.com/p/25623356)
