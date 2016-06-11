# config

webpack 初始化配置

## 通用模板（分离打包第三方库）

* 优化重合并

会生成三个js文件 app.js mobile.js vendors.js 其中 vendors.js 的名字并不是从 [name].js 配置项得来，而是从优先级更高的 CommonsChunkPlugin('vendors', 'vendors.js') 得来

* webpack.config.js

```
var path = require('path');
var webpack = require('webpack');
var nodeModulesDir = path.resolve(__dirname, 'node_modules');
var libDir = path.resolve(__dirname, 'library');

var deps = {
    'node': [
        'jquery/dist/jquery.min.js',
        'react/dist/react.min.js',
        'react-dom/dist/react-dom.min.js'
    ],
    'etc': {
        'othername': path.resolve(libDir, 'othername.js')
    }
};

var config = {
    entry: {
        vendors: [], // 需要合并打包的库
        app: path.resolve(__dirname, 'app/main.js'),
        mobile: path.resolve(__dirname, 'app/mobile.js')
    },
    resolve: {
        alias: {}, // 为 deps 里的模块定义别名，之后引用模块时不再需要使用全路径名，只使用别名即可
        extensions: ['', '.coffee', '.js'] // 文件默认后缀，依次从 无后缀 .coffee .js 这三个顺序查找匹配文件，注意：无后缀的设置必须上加上，否则会出现莫名异常
    },
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: '[name].js'
    },
    module: {
        noParse: [] // 每当 Webpack 尝试去解析那个压缩后的文件，我们阻止它，因为这不必要
    },
    plugins: [
        new webpack.optimize.CommonsChunkPlugin('vendors', 'vendors.js')
    ]
};

deps.node.forEach(function (dep) {
    let depPath = path.resolve(nodeModulesDir, dep);
    config.entry.vendors.push(depPath);
    config.module.noParse.push(depPath);
    config.resolve.alias[dep.split('/')[0]] = depPath;
});

for (let name in deps.etc) {
    let depPath = deps.etc[name];
    config.entry.vendors.push(depPath);
    config.module.noParse.push(depPath);
    config.resolve.alias[name] = depPath;
}

module.exports = config;
```

## 模块全局化

* 在模块文件内不需要引入，即可直接调用
* 值 'vue' 是在配置 resolve.alias 中映射了别名，如果没映射，就需要提供全绝对路径

```

module.exports = {
    plugins: [
      new webpack.ProvidePlugin({
          Vue: 'vue'
      })
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

## 压缩丑化

> [创建库](http://fakefish.github.io/react-webpack-cookbook/Authoring-libraries.html)

```
module.exports = {
    plugins: [
      new webpack.optimize.UglifyJsPlugin({
          compress: {
              warnings: false
          },
      })
    ]
}
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

## extensions 默认后缀

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