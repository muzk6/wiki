# vue 配置

## 解析 vue 文件的loader

* loader

`npm i --save-dev vue-loader vue-style-loader css-loader vue-html-loader`

* webpack config

```
module.loaders: [{
  test: /\.vue$/,
  loader: 'vue'
}]
```
