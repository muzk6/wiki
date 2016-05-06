# 加载 CSS 文件
> 加载 CSS 需要 css-loader 和 style-loader，他们做两件不同的事情，css-loader会遍历 CSS 文件，然后找到 url() 表达式然后处理他们，style-loader 会把原来的 CSS 代码插入页面中的一个 style 标签中

## loader

`$ npm install css-loader style-loader --save-dev`

---

## 配置
* webpack.config.js
```
module: {
loaders: [{
  test: /\.css$/, // Only .css files
  exclude: /node_modules/,
  loader: 'style!css' // Run both loaders
}]
}
```

---

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

---

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

## 内联图片
把图片转成base64字符串内联到 CSS 里来降低必要的请求数

`npm install url-loader file-loader --save-dev`

* 配置

    url-loader 传入的 limit 参数是告诉它图片如果不大于 25KB 的话要自动在它从属的 css 文件中转成 BASE64 字符串
```
module: {
    loaders: [{
      test: /\.(png|jpg)$/,
      exclude: /node_modules/,
      loader: 'url',
      query: {
          limit: 25000
      }
    }]
}
```