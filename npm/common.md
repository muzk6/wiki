# common

## 设置环境变量

* 在package.json里面的script设置环境变量，注意mac与windows的设置方式不一样
* 注意环境变量值后面不能有空格，否则会把空格当成值的一部分

* 设置

```
"scripts": {
    "publish-mac": "export NODE_ENV=prod&& webpack -p --progress --colors",
    "publish-win":  "set NODE_ENV=prod&& webpack -p --progress --colors"
}
```

* 读取(nodejs)

```
var process = require('process');
var env = process.env.NODE_ENV;
```