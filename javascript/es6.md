# es6

## 模块导入、导出

> [引用](http://es6.ruanyifeng.com/#docs/module#export命令)

* export 非默认模块，除了写法一，导出都要加上**花括号**

```
// 写法一
export var m = 1;

// 写法二
var m = 1;
export {m};

// 写法三
var n = 1;
export {n as m};

// 一起导出
var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;
export {firstName, lastName, year};

// 重命名导出
function v1() { ... }
function v2() { ... }
export {
  v1 as streamV1,
  v2 as streamV2,
  v2 as streamLatestVersion
};

// 默认导出，与写法一样
export default function foo() {
  console.log('foo');
}

// 或者写成，注意：不需要花括号
function foo() {
  console.log('foo');
}
export default foo;
```

* import 非默认模块，导入都要在**花括号**里面

```
// 分别加载
import { area, circumference } from './circle';
console.log('圆面积：' + area(4));
console.log('圆周长：' + circumference(14));

// 整体加载
import * as circle from './circle';
console.log('圆面积：' + circle.area(4));
console.log('圆周长：' + circle.circumference(14));

// 重命名导入
import { lastName as surname } from './profile';
```

* 默认导入导出比较

```
// 输出
export default function crc32() {
  // ...
}
// 输入
import crc32 from 'crc32';

// 输出
export function crc32() {
  // ...
};
// 输入
import {crc32} from 'crc32';
```

上面代码的两组写法，第一组是使用export default时，对应的import语句不需要使用大括号；第二组是不使用export default时，对应的import语句需要使用大括号。

export default命令用于指定模块的默认输出。显然，一个模块只能有一个默认输出，因此export deault命令只能使用一次。所以，import命令后面才不用加大括号，因为只可能对应一个方法。

## webpack自动加载时(new webpack.ProvidePlugin) es6 与 commonjs 的 module.exports 区别

* webapck

```
new webpack.ProvidePlugin({
    $api: 'apicloud'
})
```

```
// apicloud.js
var u = {};
u.a = 1;
u.b = 2;
export default u; //es6
module.exports = u; // commonjs

// es6 把整体导出在$api.default内
console.log($api); // {default: {a:1, b: 2}}

// commonjs 把整体导出在$api内
console.log($api); // {a:1, b: 2}
```