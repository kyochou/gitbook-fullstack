# 排序
## sort 包
sort 包原生支持 `[]int, []float64, []string`三种数据类型的排序操作.
`sort` 包的排序逻辑是通过 `sort.Interface` 接口实现的:

```go
type Interface interface {
    // 获取数据集合元素个数
    Len() int
    // 如果 i 索引的数据小于 j 索引的数据, 返回 true, 则不会调用 Swap() 方法, 即数据升序排序
    Less(i, j int) bool
    // 交换 i 和 j 索引的两个元素的位置
    Swap(i, j int)
}
```

调用 `Sort(data Interface)` 方法进行排序操作.
`IsSorted(data Interface) bool` 返回数据集是否已经排好顺序.
`Reverse(data Interface) Interface` 方法可以将数据进行逆序排序: `sort.Sort(sort.Reverse(target))`.
`Search(n int, f func(int) bool) int` 方法会使用 "二分查找" 算法来找出能使 `f(x)(0<=x<n)` 返回 true 的最小值 i. 前提条件是 `f(x)(0<=x<i)`均返回 false, `f(x)(i<=x<n)` 均返回 true. 如果不存在 i 可以使 f(i) 返回 true, 则返回 n.
`Search()` 函数的一个常用场景是搜索元素是否存在于已经升序排好的切片中.
