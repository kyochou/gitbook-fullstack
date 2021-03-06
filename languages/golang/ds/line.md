# Line

## Array
数组是一个由固定长度的特定类型组成的序列.
类型 `[n]T` 表示拥有 n 个 T 类型的值的数组.
如果一个数组的元素类型是可以相互比较的, 那么这个数组也是可比较的.

### Definition

```go
// 一般定义
var b = [6]int{2, 3, 5, 7, 11, 13}

// 元素初始化为其对应类型的零值
var a [10]int 

// 如果长度是 `...`, 则表示数组的长度根据值来定
d := [...]int{1, 2}

// 指定值对应的索引
type Currency int
const (
    USD Currency = iota // 美元
    EUR                 // 欧元
    GBP                 // 英镑
    RMB                 // 人民币
)
symbol := [...]string{USD: "$", EUR: "€", GBP: "￡", RMB: "￥"}

// 定义了一个含有 10 个元素的数组，最后一个元素被初始化为 -1，其它元素都是用零值初始化
r := []int{9: -1}
// 定义了一个含有 10 个元素的数组，前两个元素为 1, 2; 最后一个元素被初始化为 -1，其它元素都是用零值初始化.
r := []int{1, 2, 9: -1}
```


### FAQs
* 注意, 如果数据初始化时长度不是常量, 需使用 make 函数创建切片而不是使用数组:

    ```go
    // 这种写法是错误的
    var array [len(arr)]int
    // 应使用 make 函数
    array := make([]int, len(arr))
    
    ```
    
    
## container/list
list 包实现了一个双链表(doubly linked list).

## container/ring
环的尾部就是头部. 所以环的每个元素实际上可以代表其自身.

