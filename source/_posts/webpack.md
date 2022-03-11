---
title: 前端工程化
date: 2022-02-17 14:50:02
cover: https://s4.ax1x.com/2022/02/17/H5dRjx.png
tags:
- webpack
- commonJs
---

## 前端工程化

### commonJs规范
1、一个文件就是一个模块。  
2、每个模块都有单独的作用域。  
3、通过module.exports导出成员。  
4、通过require函数载入模块。

commonJs以同步的方式加载模块，适用于node环境使用

### AMD(Asynchronous Module Definition)规范
1、使用define声明模块  
```javascript
define('module1', ['jquery', './module2'], function ($, module2) {
    return {
        start: function () {
            $('body').animate({margin: '200px'})
            module2()
        }
    }
})
```
2、使用require加载模块
```javascript
require(['./module1'], function (module1) {
    module1.start();
})
```
3、劣势  
AMD使用起来相对复杂、模块化较多则有频繁的JS请求
4、实现  
require.js

### CMD(Common Module Definition)规范
CMD规范与AMD类似，尽量保持简单，并与CommonJs和Node.js的modules规范保持了很大的兼容性
```javascript
define(function (require, exports, module) {
    var $ = require('jquery')
    var spinning = require('./spinning')
    exports.doSomething = function () {
        console.log('do something')
    }
    module.exports = take;
})
```
优点：依赖就近，延迟执行，可以很容易在Node.js中执行  
缺点：依赖SPM打包，模块加载的逻辑偏重  
实现：Sea.js

### 现代模块化规范标准
浏览器环境下遵循ES Modules规范  
node环境下遵循CommonJs规范

### ES Modules规范
ES Modules是由es2015(es6)提出的模块化规范，现在已被大部分浏览器兼容
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<!--通过给script添加type = module 的属性，就可以以ES Module的标准执行其中的js代码-->
  <script type="module">
    console.log('this is es module')
  </script>
<!--1、ESM自动采用严格模式，忽略 'use strict'-->
<script type="module">
  // 严格模式下this为undefined， 非严格执行window
  console.log(this)
</script>

<!--2、每个 ESM 都是运行在单独的私有作用域中-->
<script type="module">
  var foo = 123;
  console.log(foo)
</script>
<script type="module">
  console.log(foo)
</script>

<!--3、ESM 是通过 CORS的方式请求外部js模块的,如果外部js不支持CORS，则会报跨域错误-->
<script type="module" src="xxx.js"></script>

<!--4、ESM 的 script 标签会延迟执行脚本，会等网页渲染完成再执行 类似defer属性-->
<script type="module" src="./demo.js"></script>
<p>123456</p>

</body>
</html>
```

### webpack配置
```javascript
const path = require('path');
const webpack = require('webpack');
const {CleanWebpackPlugin} = require('clean-webpack-plugin');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const OptimizeCSSAssetsPlugin = require('optimize-css-assets-webpack-plugin');
const TerserWebpackPlugin = require('terser-webpack-plugin');
module.exports = {
    entry: {
        main: './src/main.js'
    },
    output: {
        filename: 'bundle_[name].js',
        path: path.jion(__dirname, dist)
    },
    module: {
        rules: [
            {
                test: /\.js$/,
                use: {
                    loader: 'babel-loader',
                    options: {
                        presets: ['@babel/preset-env']
                    }
                }
            },
            {
                test: /\.css$/,
                use: ['css-loader']
            }
        ]
    },
    plugins: [
        new CleanWebpackPlugin(),
        new HtmlWebpackPlugin({
            title: '咪咕视频',
            root: 'app',
            template: './template/template.js',
            meta: {
                viewport: 'width-device-width'
            },
            chunk: ['index']
        }),
        new HtmlWebpackPlugin({
            title: '咪咕影院',
            filename: 'migumovie.html',
            root: 'app',
            template: './template/template.js',
            meta: {
                viewport: 'width-device-width'
            },
            chunk: ['main']
        }),
        new webpack.HotModuleReplacementPlugin()
    ]
}
```
一、webpack如何实现代码分离  
1、入口起点：使用entry配置手动的代码分离。
2、防止重复：使用CommonsChunkPlugin去重和分离chunk。
3、动态导入：通过模板的内联函数调用来分离代码。

二、常见的webpack loader  
loader:是一个导出函数的JavaScript模块，根据rule匹配文件扩展名，处理文件的转化器。

file-loader: 把文件输出到一个文件夹中，在代码通过相对URL去引用输出的文件（处理图片和字体）。

url-loader: 与file-loader类似，区别是用户能够设置一个阈值，大于阈值就会交给file-loader处理，小于阈值时返回文件base64形式编码（处理图片和字体）。

image-loader: 加载并压缩图片文件。

babel-loader: 将es6转化成es5。

sass-loader: 将scss/sass转化成css。

less-loader: 将less转化为css。

css-loader: 加载css,支持模块化、压缩、文件导入等特性。

postcss-loader: 扩展css语法，使用下一代的css，可以配合autoprefixer插件自动补齐css3前缀。

eslint-loader: 通过eslint检查JavaScript代码。

三、常见的webpack plugin  
plugin：本质是插件，基于事件流框架Tapable，插件可以扩展webpack的功能，在webpack运行的生命周期中会广播出许多事件，plugin可以监听这些事件，在合适的时机通过webpack提供的API改变输出的结果。

html-webpack-plugin: 创建html。

clean-webpack-plugin：目录清除。

uglifyjs-webpack-plugin：压缩js文件。

mini-css-extract-plugin：分离样式文件，CSS 提取为独立文件，支持按需加载。

```javascript
// 实现一个webpack plugin
class CleanZhuSiPulgin {
    apply(compiler) {
        compiler.hooks.emit.tap('cleanZhuSiPlugin', (compilation) => {
            for (let name in compilation.assets) {
                console.log(name);
            }
        })
    }
}

module.exports = {
    entry: {},
    output: {},
    module: {
        rules: []
    },
    plugins: [
        new CleanZhuSiPulgin()
    ]
}
```