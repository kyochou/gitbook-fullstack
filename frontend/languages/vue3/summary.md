# Vue3

## 语法
### 响应式
通过 `ref` 定义的值在模板中可以直接访问. 但在 js 中需要通过 `.value` 的方式才能访问.
对于引用类型的对象, 优先使用 `reactive`. 
`ref` 在的使用场景:
```ts
// 基本类型的响应式化
const num = ref(1)

// 提前将变量声明为响应式
const form = ref()

```

#### Refs
* [ref及reactive的区别及本质](https://juejin.cn/post/7013326406444646407)
* [vue3 还不知道怎么选ref和reactive,还不赶快进来](https://juejin.cn/post/7124864087359488014)

### 组件
