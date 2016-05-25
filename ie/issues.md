# ie

## 缺少标识符

* 原因

用了js关键字作为对象键

* 示例

```
var Jpy = {
    switch: function() {}, // error
    abc: function() {}
}

Jpy.swithc(); // error
Jpy.abc();
```

* 解决方案

除了不使用关键字作为键，把键用引号括起来，并且在调用是以取关联数组值的方式来调用

```
var Jpy = {
    'switch': function() {},
    abc: function() {}
}

Jpy['swithc']();
Jpy.abc();
```