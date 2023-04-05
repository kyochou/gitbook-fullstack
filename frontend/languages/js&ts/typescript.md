# Typescript

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
