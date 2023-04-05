# Element UI

## Checkbox
* 使用 `value` 属性而不是 `checked` 来控制选中状态的改变. 参考: [el-checkbox 通过 vuex 修改 checked checked 状态没有改变 #3619](https://github.com/ElemeFE/element/issues/3619).
* 为 `change` 方法传递额外的参数: `@change="checked => methodname(checked, args)"`. 参考: [elementUI 里 CheckBox 组件的 change 回调如何在使用自定义传参的条件下保留默认传参？](https://segmentfault.com/q/1010000014583061).


