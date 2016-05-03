# babel-conf
> babel 配置

## polyfill
> 打包时自动polyfill 不用在浏览器中额外载入以污染全局
> [babel-plugin-transform-runtime](https://www.npmjs.com/package/babel-plugin-transform-runtime#via-babelrc-recommended)

* Installation

`$ npm install babel-plugin-transform-runtime`

* Usage

`plugins: ["transform-runtime"]`

---

## IE8 兼容性
> ie8 ie-8 IE8 IE-8 兼容性 compatibility
1. [es3-member-expression-literals](http://babeljs.io/docs/plugins/transform-es3-member-expression-literals/)
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

2. [es3-property-literals](http://babeljs.io/docs/plugins/transform-es3-property-literals/)
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