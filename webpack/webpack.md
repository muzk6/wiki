# webpack

#### 自动打包
> [webpack-dev-server](http://fakefish.github.io/react-webpack-cookbook/Running-a-workflow.html)

`webpack-dev-server --devtool eval --progress --colors --hot --content-base build`

* webpack-dev-server - 在 localhost:8080 建立一个 Web 服务器
* --devtool eval - 为你的代码创建源地址。当有任何报错的时候可以让你更加精确地定位到文件和行号
* --progress - 显示合并代码进度
* --colors - Yay，命令行中显示颜色！
* --content-base build - 指向设置的输出目录