# babel-conf

babel 配置

## polyfill

> [babel-plugin-transform-runtime](https://www.npmjs.com/package/babel-plugin-transform-runtime#via-babelrc-recommended)

打包时自动polyfill 不用在浏览器中额外载入以污染全局

* Installation

`$ npm install babel-plugin-transform-runtime`

* Usage

`plugins: ["transform-runtime"]`

## IE8 兼容性

ie8 ie-8 IE8 IE-8 兼容性 compatibility

* [es3-member-expression-literals](http://babeljs.io/docs/plugins/transform-es3-member-expression-literals/)
```
In
    foo.catch;
Out
    foo["catch"];
```
`$ npm install --save-dev babel-plugin-transform-es3-member-expression-literals`
```
{
  "plugins": ["transform-es3-member-expression-literals"]
}
```

* [es3-property-literals](http://babeljs.io/docs/plugins/transform-es3-property-literals/)
```
In
    var foo = {
      catch: function () {}
    };
Out
    var foo = {
      "catch": function () {}
    };
```
`$ npm install --save-dev babel-plugin-transform-es3-property-literals`
```
{
  "plugins": ["transform-es3-property-literals"]
}
```

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