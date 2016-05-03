# babel

## webpack Unexpected token <

> [babel-loader jsx SyntaxError: Unexpected token](http://stackoverflow.com/questions/33460420/babel-loader-jsx-syntaxerror-unexpected-token)

`npm install --save-dev babel-preset-react`
```
{
    test: /\.js$/,
    exclude: /node_modules/,
    loader: "babel",
    query:
     {
       presets:['react']
     }
}
```

---

## Unexpected token import
`npm install --save-dev babel-preset-es2015`
```
{
    query:
     {
       presets:['es2015']
     }
}
```