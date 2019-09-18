# Array
## Indexed Array

```js
// 删除指定元素
const index = arr.indexOf(val)
arr.splice(index, 1)

// 比较数据元素是否相同(次序无关)
_.isEqual(arr1.sort(), arr2.sort())
```

## JSON
* pretty-print JSON: `JSON.stringify(obj, null, 2)`. [How can I pretty-print JSON using JavaScript?](https://stackoverflow.com/questions/4810841/how-can-i-pretty-print-json-using-javascript)