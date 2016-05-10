# config

webpack 初始化配置

## 分离打包第三方框架

* 优化重合并

会生成三个js文件 app.js mobile.js vendors.js 其中 vendors.js 的名字并不是从 [name].js 配置项得来，而是从优先级更高的 CommonsChunkPlugin('vendors', 'vendors.js') 得来

```
var path = require('path');
var webpack = require('webpack');
var nodeModulesDir = path.resolve(__dirname, 'node_modules');

var deps = [
    'jquery/dist/jquery.min.js',
    'react/dist/react.min.js',
    'react-dom/dist/react-dom.min.js'
];

var config = {
    entry: {
        app: path.resolve(__dirname, 'app/main.js'),
        mobile: path.resolve(__dirname, 'app/mobile.js'),
        vendors: [] // 需要合并打包的库
    },
    resolve: {
        alias: {} // 每当 "react" 在代码中被引入，它会使用压缩后的 React JS 文件，而不是到 node_modules 中找
    },
    output: {
        path: path.resolve(__dirname, './dist'),
        filename: '[name].js'
    },
    module: {
        noParse: [] // 每当 Webpack 尝试去解析那个压缩后的文件，我们阻止它，因为这不必要
    },
    plugins: [
        new webpack.optimize.CommonsChunkPlugin('vendors', 'vendors.js')
    ]
};

deps.forEach(function (dep) {
    var depPath = path.resolve(nodeModulesDir, dep);
    config.entry.vendors.push(depPath);
    config.module.noParse.push(depPath);
    config.resolve.alias[dep.split(path.sep)[0]] = depPath;
});
```

## 压缩丑化

> [创建库](http://fakefish.github.io/react-webpack-cookbook/Authoring-libraries.html)

```
module.exports = {
    plugins: [
      new webpack.optimize.UglifyJsPlugin({
          compress: {
              warnings: false
          },
      }),
    ]
}
```

## webpack 命令行的几种基本命令

```
$ webpack // 最基本的启动webpack方法
$ webpack -w // 提供watch方法，实时进行打包更新
$ webpack -p // 对打包后的文件进行压缩，提供production
$ webpack -d // 提供source map，方便调试。
$ webpack --colors // 输出结果带彩色，比如：会用红色显示耗时较长的步骤
$ webpack --profile // 输出性能数据，可以看到每一步的耗时
$ webpack --display-modules // 默认情况下 node_modules 下的模块会被隐藏，加上这个参数可以显示这些被隐藏的模块
```

## publicPath 配置

给自动生成的URL加上的“前缀”

* 插件 extract-text-webpack-plugin 生成独立CSS文件时的URL例子
* 第三个例子的URL就是连起来的，之间没 / 隔开，所以 publicPath 可以理解成一个单纯的 URL 字符串前缀

| publicPath | URL |
| :---: | :---: |
| /dir/ | /dir/style.css |
| ./dir/ | ./dir/style.css |
| ./dir | ./dirstyle.css |


## 静态资源服务器

> [webpack-dev-server](http://fakefish.github.io/react-webpack-cookbook/Running-a-workflow.html)

除了提供模块打包功能，Webpack还提供了一个基于Node.js Express框架的开发服务器，它是一个静态资源Web服务器，对于简单静态页面或者仅依赖于独立服务的前端页面，都可以直接使用这个开发服务器进行开发。在开发过程中，开发服务器会监听每一个文件的变化，进行实时打包，并且可以推送通知前端页面代码发生了变化，从而可以实现页面的自动刷新

`webpack-dev-server --devtool eval --progress --colors --hot --content-base build`

* webpack-dev-server - 在 localhost:8080 建立一个 Web 服务器
* --devtool eval - 为你的代码创建源地址。当有任何报错的时候可以让你更加精确地定位到文件和行号
* --progress - 显示合并代码进度
* --colors - Yay，命令行中显示颜色！
* --content-base build - 指向设置的输出目录

## 设置环境变量

* 在package.json里面的script设置环境变量，注意mac与windows的设置方式不一样
* 注意环境变量值后面不能有空格，否则会把空格当成值的一部分

```
"scripts": {
    "publish-mac": "export NODE_ENV=prod&& webpack -p --progress --colors",
    "publish-win":  "set NODE_ENV=prod&& webpack -p --progress --colors"
}
```

## 查找依赖

Webpack 是类似 Browserify 那样在本地按目录对依赖进行查找的 可以构造一个例子, 用 --display-error-details 查看查找过程, 例子当中 resolve.extensions 用于指明程序自动补全识别哪些后缀, 注意一下, extensions 第一个是空字符串! 对应不需要后缀的情况

* webpack.config.js
```
module.exports = {
    entry: './a.js',
    output: {
    filename: 'b.js'
    },
    resolve: {
    extensions: ['', '.coffee', '.js']
    }
}
```

* a.js

./c 是不存在, 从这个错误信息当中我们大致能了解 Webpack 是怎样查找的 大概就是会尝试各种文件名, 会尝试作为模块, 等等 一般模块就是查找 node_modules, 但这个也是能被配置的:

`require('./c')`

```
➤➤ webpack --display-error-details
Hash: e38f7089c39a1cf34032
Version: webpack 1.5.3
Time: 54ms
Asset  Size  Chunks             Chunk Names
 b.js  1646       0  [emitted]  main
   [0] ./a.js 15 {0} [built] [1 error]

ERROR in ./a.js
Module not found: Error: Cannot resolve 'file' or 'directory' ./c in /Users/chen/Drafts/webpack/details
resolve file
  /Users/chen/Drafts/webpack/details/c doesn't exist
  /Users/chen/Drafts/webpack/details/c.coffee doesn't exist
  /Users/chen/Drafts/webpack/details/c.js doesn't exist
resolve directory
  /Users/chen/Drafts/webpack/details/c doesn't exist (directory default file)
  /Users/chen/Drafts/webpack/details/c/package.json doesn't exist (directory description file)
[/Users/chen/Drafts/webpack/details/c]
[/Users/chen/Drafts/webpack/details/c.coffee]
[/Users/chen/Drafts/webpack/details/c.js]
 @ ./a.js 2:0-14
```