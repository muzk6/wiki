# apicloud

## 真机同步时提示找不到模块

把代码提交上去，除了编译自定义 loader 外，还要进行一次云编译，云编译过后要更新下代码

## 真机同步或云编译时很慢

进行真机同步或云编译的项目文件夹，不能有过多无用的文件，否则真机同步时连里面的文件也
一起扫描，导致以上操作过超慢。如果用 nodejs 模式开发，需把 apicloud 项目放到 widget
文件夹里， node_modules 独立出来与 widget 同级，以避免扫描过多文件影响同步和云编译
速度

## 以模块化模式开发时提示 apiready 未定义

用 webpack 打包成模块化脚本时，apiready 必须要加上 window 前缀，即 window.apiready