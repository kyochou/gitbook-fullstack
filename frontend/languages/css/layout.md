# 布局

## 文档流
HTML 元素分为块级元素和行内元素. 块级元素在浏览器中会占满容器的宽度. 后面的元素会在新的一行进行排列.
正常文档流(Normal Flow)的方向是自上到下, 自左到右.

脱离文档流:
* `float` 属性可以使元素脱离文档流.
* `position: absolute` 属性会使元素脱离文档流. 通常会将其父类元素的 `position` 值设置为 `relative` 以配合定位.
* `position:fixed` 可基于浏览器可视区域进行进行偏移和定位, 并且永久保持在那个位置.
* `position:sticky` 会在浏览器滚动到设置的偏移位置后, 脱离文档流, 并保持在那个位置.