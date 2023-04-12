# Typescript

## FAQs

##### 使用第三方组件时提示变量未定义(如 wx)

在 `.eslintrc` 文件中添加全局变量声明: 

```ts
globals: {
    wx: true,
},
```

为变量定义声明文件(名称要以变量名开头, 如 `wx.d.ts`):

```ts
declare const wx: any
```

## Object

Clone 对象的几种方式: `Spread, Object.assign, JSON, _.DeepClone`. 参考 [3 Ways to Clone Objects in JavaScript](https://www.samanthaming.com/tidbits/70-3-ways-to-clone-objects/).  
```ts

/* Shallow Clone */
// "Spread"
{ ...food }
// "Object.assign"
Object.assign({}, food)

/* DeepClone */
// "JSON"
JSON.parse(JSON.stringify(food))
// lodash.clonedeep
lodash.clonedeep(food)

```
