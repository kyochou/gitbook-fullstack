# String

## byte[] <=> string
基于 [Encoding](https://encoding.spec.whatwg.org/) 标准可在字节数组与字符串之间转换. 浏览器如果不支持可使用兼容库 [inexorabletash/text-encoding](https://github.com/inexorabletash/text-encoding).

```js
const bytes = new TextEncoder('utf-8').encode('中国人')
const str = new TextDecoder('utf-8').decode(bytes)
```

静态方法 `String.fromCharCode` 只能用来解析 UTF-16 编码的字节值.
```js
// ABC
String.fromCharCode(65, 66, 67) 
String.fromCharCode.apply(null, [65, 66, 67]) // 数组参数
```