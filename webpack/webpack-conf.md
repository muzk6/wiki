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
> 优化重合并
```
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
        vendors: vendors.names // 需要合并打包的库
    },
    resolve: {
        alias: vendors.aliases // 每当 "react" 在代码中被引入，它会使用压缩后的 React JS 文件，而不是到 node_modules 中找
    },
    output: {
        path: path.resolve(__dirname, './dist'),
        filename: 'app.js'
    },
    module: {
        noParse: vendors.paths // 每当 Webpack 尝试去解析那个压缩后的文件，我们阻止它，因为这不必要
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