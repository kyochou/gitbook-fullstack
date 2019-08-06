# 树


## container/heap
包 heap 为所有实现了 `heap.Interface` 的类型提供堆操作.
一个堆即一棵有序的树. 这棵树的每个节点的值都比它的子节点的值要小(大), 而整棵树最小(大)的值位于树根部, 也即索引 0 的位置.
堆是实现优先队列的一种常见方法.

### heap.Interface

```go
type Interface interface {
    sort.Interface
    Push(x interface{}) // add x as element len().
    Pop() interface{} // remove and return element len() - 1.
}
```

### Ops
* `Init(h Interface)`: 在执行任何堆操作之前, 必须对堆 h 进行初始化.
* `Push(h Interface, x interface{})`: 将值 x 推入到堆 h 中.
* `Pop(h Interface)`: 自堆 h 中移除堆顶元素(根结点)并返回. 此操作会保证堆顶为排序后的元素.
* `Fix(h Interface, i int)`: 在索引 i 上的元素的值发生变化后, 重新修复堆 h 的有序性.
* `Remove(h Interface, i int) interface{}`: 移除堆 h 中索引为 i 的元素并返回.
