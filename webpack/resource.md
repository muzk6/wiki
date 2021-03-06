# 加载静态资源

## 样式

加载 CSS 需要 css-loader 和 style-loader，他们做两件不同的事情，css-loader会遍历 CSS 文件，然后找到 url() 表达式然后处理他们，style-loader 会把原来的 CSS 代码插入页面中的一个 style 标签中

`npm install --save-dev css-loader style-loader`

* 直接嵌入到html的style元素中

```
module: {
    loaders: [{
        test: /\.css$/, // 只对css后缀的文件
        loaders: ['style', 'css'] // 从右到左依次执行，顺序不能顺序换
    }]
}
```

* 独立出CSS文件来

`npm install --save-dev extract-text-webpack-plugin`

```
var ExtractTextPlugin = require("extract-text-webpack-plugin");

module: {
    loaders: [{
        test: /\.css$/,
        loader: ExtractTextPlugin.extract('style', 'css')
    }],
    plugins: [
        new ExtractTextPlugin('style.css', {allChunks: true}) // 合成一个文件
    ]
}
```

## 图片

把图片转成base64字符串内联到 CSS 里来降低必要的请求数

url-loader 传入的 limit 参数是告诉它图片如果不大于 10KB 的话要自动在它从属的 css 文件中转成 BASE64 字符串

* loader

`npm i --save-dev url-loader file-loader`

* setting

```
module: {
    loaders: [{
        test: /\.(jpe?g|png|gif|svg)$/i,
        loader: 'url?limit=10000&name=img/[hash:8].[name].[ext]'
    }]
}
```

## 字体

`npm i --save-dev url-loader file-loader`

```
module: {
    loaders: [{
        test: /\.(woff|eot|ttf)$/i,
        loader: 'url?limit=10000&name=fonts/[hash:8].[name].[ext]'
    }]
}
```

## 组件式加载

* app/components/MyComponent.css

```
.MyComponent-wrapper {
  background-color: #EEE;
}
```

* app/components/MyComponent.jsx

```
import './MyComponent.css';
import React from 'react';

export default React.createClass({
  render: function () {
    return (
      <div className="MyComponent-wrapper">
        <h1>Hello world</h1>
      </div>
    )
  }
});
```

## 使用内联样式取代 CSS 文件

```
import React from 'react';

var style = {
  backgroundColor: '#EEE'
};

export default React.createClass({
  render: function () {
    return (
      <div style={style}>
        <h1>Hello world</h1>
      </div>
    )
  }
});
```