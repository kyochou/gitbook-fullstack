# API
## 幂等性
传统应用中的接口调用, 只有**成功(true)**和**失败(false)**两种状态. 但在微服务中, 还需要处理**超时(unknown/undefined)**的情况.
幂等性强调的是外界通过接口对系统内部造成的影响, 外界怎么看系统和幂等性没有关系, **需要保证的是同一接口的一次或多次调用对某一个资源具有相同的副作用. 但是返回值允许不同**.

### 数据操作
* 查询: 查询操作并不会产生数据的变更, 因此天然具有幂等性.
* 删除: 数据的实际删除操作只会进行一次, 因此也是幂等的.
* 增加:
    * 如果使用了唯一索引, 重复插入会导致错误. 因此是幂等的. 可使用 MySQL 中的 `insert ignore into`, `replace`, `on duplicate key update` 来避开索引冲突时的错误.
    * 不使用唯一索引会重复插入数据, 所以非幂等.
* 修改:
    * 计算式 Update, 如 `update table set number = number -1 where id = 1` 这类 SQL 操作, 是非幂等的.
    * 非计算式 Update, 如 `update table set number = 3 where id = 1` 这类 SQL 操作, 是幂等的.


### Refs
* [小说：白话幂等性设计](https://mp.weixin.qq.com/s/n_cfjUwivgIdIY0Ttkv_sQ)