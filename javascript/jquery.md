# jQuery

## $.fn.css() 取样式值相关问题
只能取style属性里的样式，不能取class里的样式

---

## 位置偏移量
以下几个的区别：

* $.fn.offset
* dom.offsetLeft / dom.offsetTop
* style.left / style.top

区别：

1. $.fn.offset 和 dom.offset... 只有在该元素不隐藏的前提下才有值，否则都为0
2. $.fn.offset 总是相对于body的位移
3. dom.offsetLeft -right 相对于最近的含有offsetParent属性父元素(通过 本元素dom.offsetParent 即可得出)，其原理基本与style位移规则一致
4. style.left -right 只有设置了才有值，否则为auto