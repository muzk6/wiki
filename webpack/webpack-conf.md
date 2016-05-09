# config

webpack 初始化配置

## 分离打包第三方框架

* 优化重合并

会生成三个js文件 app.js mobile.js vendors.js 其中 vendors.js 的名字并不是从 [name].js 配置项得来，而是从优先级更高的 CommonsChunkPlugin('vendors', 'vendors.js') 得来

```
'use strict';

var path = require('path');
var webpack = require('webpack');
var nodeModulesDir = path.resolve(__dirname, 'node_modules');

var vendors = ((mods) => {
    let vendors = {};
    vendors.aliases = [];
    vendors.names = [];
    vendors.paths = [];

    mods.forEach((mod) => {
        let name = mod.name;
        let relPath = path.resolve(nodeModulesDir, mod.path);

        vendors.names.push(name);
        vendors.paths.push(relPath);

        let alias = {};
        alias[name] = relPath;
        vendors.aliases.push(alias);
    });

    return vendors;
})([
    {name: 'jquery', path: 'jquery/dist/jquery.min.js'},
    {name: 'react', path: 'react/dist/react.min.js'},
    {name: 'react-dom', path: 'react-dom/dist/react-dom.min.js'}
]);

module.exports = {
    entry: {
        app: path.resolve(__dirname, 'app/main.js'),
        mobile: path.resolve(__dirname, 'app/mobile.js'),
        vendors: vendors.names // 需要合并打包的库
    },
    resolve: {
        alias: vendors.aliases // 每当 "react" 在代码中被引入，它会使用压缩后的 React JS 文件，而不是到 node_modules 中找
    },
    output: {
        path: path.resolve(__dirname, './dist'),
        filename: '[name].js'
    },
    module: {
        noParse: vendors.paths // 每当 Webpack 尝试去解析那个压缩后的文件，我们阻止它，因为这不必要
    },
    plugins: [
        new webpack.optimize.CommonsChunkPlugin('vendors', 'vendors.js')
    ]
};
```

## 浏览器自动刷新

> [webpack-dev-server](http://fakefish.github.io/react-webpack-cookbook/Running-a-workflow.html)

`webpack-dev-server --devtool eval --progress --colors --hot --content-base build`

* webpack-dev-server - 在 localhost:8080 建立一个 Web 服务器
* --devtool eval - 为你的代码创建源地址。当有任何报错的时候可以让你更加精确地定位到文件和行号
* --progress - 显示合并代码进度
* --colors - Yay，命令行中显示颜色！
* --content-base build - 指向设置的输出目录

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