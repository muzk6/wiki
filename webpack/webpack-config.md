# config
> webpack 初始化配置

#### react + es6
> npm dependencies

`npm install --save-dev babel-loader babel-core babel-preset-react babel-preset-es2015`
> webpack.config.js
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

#### IE8 兼容性
https://github.com/reactjs/react-redux/pull/135
https://github.com/zloirock/core-js
https://github.com/reactjs/react-redux/issues/227

> [es3-member-expression-literals](http://babeljs.io/docs/plugins/transform-es3-member-expression-literals/)
```
In
    foo.catch;
Out
    foo["catch"];
```
`$ npm install babel-plugin-transform-es3-member-expression-literals`
```
{
  "plugins": ["transform-es3-member-expression-literals"]
}
```

> [es3-property-literals](http://babeljs.io/docs/plugins/transform-es3-property-literals/)
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
`$ npm install babel-plugin-transform-es3-property-literals`
```
{
  "plugins": ["transform-es3-property-literals"]
}
```