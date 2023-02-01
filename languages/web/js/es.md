# ES

## Practices

```javascript

# 不要使用 (, [, or ` 等作为一行的开始. 在没有分号的情况下代码压缩后会导致报错. 可以通过在其前加 ; 以避免报错.

const message = `Hello ${name}` // 字符串模板
let score = val || 0 // eq let score = val ? val : 0 
```
