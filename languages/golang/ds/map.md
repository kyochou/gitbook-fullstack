# Map
注意, Go map 的遍历输出是无序的.
在 Go 语言中, 一个 map 就是一个哈希表的引用, map 类型可以写成 `map[K]V`, 其中 K,V 分别对应哈希表的键和值. 其中 K 对应的 key 必须是支持 `==` 比较运算符的数据类型(不能为函数, 字典, 切片类型), map 通过测试 key 是否相等来判断是否已经存在. 
map 的键值可以是 `interface{}` 类型, 但这是很危险的.
通常宽度越小的类型(类型的宽度是指它的单个值需要占用的字节数)求哈希的操作越快. 求哈希和判等操作的速度越快, 对应的类型越适合作为键类型.
map 将键映射到值. map 的零值为 `nil`. `nil` 既没有键, 也不能添加键.
`make` 函数会返回给定类型的映射, 并将其初始化备用.
使用 `delete` 函数删除 map 元素.
map 中的元素并不是一个变量, 因此我们不能对其进行取址操作.
和 slice 一样, map 之间也不能进行相等比较, 唯一的例外是和 nil 进行比较.
Go 语言中并没有提供 set 类型, 但是 map 的 key 也是不相同的, 可以用 map 实现类似 set 的功能.
map 的遍历顺序是随机的. 如果要按顺序遍历 key/value 对, 必须显式地对 key 进行排序.
除了添加键 `_` 元素外, 我们对一个值为 `nil` 的字典上做任何操作都不会引起错误.
```go
// 顺序遍历 map
import `sort`

var names := make([]string, 0, len(ages))
for name := range ages {
    names = append(names, name)
}
sort.Strings(names)
for _, name := range names {
    fmt.Printf("%s\t%d\n", name, ages[name])
}

```

map 操作:
```go
// make 创建
ages0 := make(map[string]int)
ages0[`kyo`] = 31
// 直接创建
ages1 := map[string]int{
    `kyo`: 31,
}
// 删除
delete(ages0, `kyo`)
// 遍历
for name, age := range ages0 {
    // ...
]
// 是否存在
if name, exists := ages0[`kyo`]; exists {
    // ...
} 
```

## sync.Map
`sync.Map` 在读多写少时性能会比较好.

### Refs
* [Go 1.9 sync.Map揭秘](https://colobu.com/2017/07/11/dive-into-sync-Map/)
* [orcaman/concurrent-map](https://github.com/orcaman/concurrent-map/blob/master/README-zh.md)
