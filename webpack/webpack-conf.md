# config
> webpack 初始化配置

## react + es6
* npm dependencies

`npm install --save-dev babel-loader babel-core babel-preset-react babel-preset-es2015 react react-dom`

* webpack.config.js

```
var path = require('path');
var config = {
    entry: path.resolve(__dirname, 'app/main.js'),
    output: {
        path: path.resolve(__dirname, 'build'),
        filename: 'bundle.js'
    },
    module: {
        loaders: [{
            test: /\.jsx?$/, // 用正则来匹配文件路径，这段意思是匹配 js 或者 jsx
            exclude: /node_modules/,
            loader: 'babel', // 加载模块 "babel" 是 "babel-loader" 的缩写
            query: {
                presets: ['react', 'es2015']
            }
        }]
    }
};

module.exports = config;
```

---

## 分离打包第三方框架
> 分离 jquery
```
var path = require('path');
var webpack = require('webpack');

module.exports = {
    entry: {
        app: path.resolve(__dirname, 'app/main.js'),
        vendors: ['jquery']
    },
    output: {
        path: path.resolve(__dirname, './dist'),
        filename: 'app.js'
    },
    plugins: [
        new webpack.optimize.CommonsChunkPlugin('vendors', 'vendors.js')
    ]
};
```

---

## 浏览器自动刷新
> [webpack-dev-server](http://fakefish.github.io/react-webpack-cookbook/Running-a-workflow.html)

`webpack-dev-server --devtool eval --progress --colors --hot --content-base build`

* webpack-dev-server - 在 localhost:8080 建立一个 Web 服务器
* --devtool eval - 为你的代码创建源地址。当有任何报错的时候可以让你更加精确地定位到文件和行号
* --progress - 显示合并代码进度
* --colors - Yay，命令行中显示颜色！
* --content-base build - 指向设置的输出目录