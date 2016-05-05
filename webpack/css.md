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