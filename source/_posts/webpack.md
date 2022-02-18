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