---
title: htmlAndCss知识
date: 2022-01-10 14:37:16
tags: 
- html
cover: https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fn.sinaimg.cn%2Fsinakd20123%2F600%2Fw1920h1080%2F20210730%2F2026-bbf1e288e62fde3aa78a69e6da74dea0.jpg&refer=http%3A%2F%2Fn.sinaimg.cn&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1644388782&t=a875106d13ed8d655541100a3419694e
---

## htmlAndCss知识
### 1、html5新特性、语义化
html5语义化指的是合理正确的使用语义化的标签来创建页面结构，如header、footer、nav,从标签上即可以直观的知道这个标签的作用  
语义化标签: header、nav、main、article、section、aside、footer  
语义化的优点有：  
1.代码结构清晰，易于阅读，利与开发和维护  
2.方便其他设备解析（如屏幕阅读器）根据语义渲染网页  
3.有利于搜索引擎优化（SEO），搜索引擎爬虫会根据不同的标签来赋予不同的权重  

### 2、浏览器渲染机制、重绘、重排
#### 网页生产过程：  
1.HTML被HTML解析器解析成DOM树  
2.CSS则被CSS解析器解析成CSSOM树  
3.结合DOM树和CSSOM树，生成一颗渲染树（Render Tree）  
4.生成布局（flow）,即将所有渲染树的所有节点进行平面合成  
5.将布局绘制（paint）在屏幕上

#### 重排（回流）
当DOM的变化影响了元素的几何信息（DOM对象的位置和尺寸大小），浏览器需要重新计算元素的几何属性，将其安放在界面中的正确位置，这个过程叫做重排。  
触发：1.添加或者删除可见的DOM元素,2.元素尺寸改变---边距、填充、边框、宽度和高度

#### 重绘
当DOM元素的外观发生改变，但没有改变布局，重新把元素外观绘制出来的过程，叫做重绘。  
触发：改变元素的color、background、box-shadow等属性

#### 重排优化建议
1.分离读写操作  
2.样式集中修改  
3.缓存需要修改的DOM元素  
4.尽量只修改position:absolute或fixed元素，对其他元素影响不大  
5、动画开始GPU加速，translate使用3D变化  

transform不重绘，不回流 是因为transform属于合成属性，对合成属性进行transition/animate动画时，将会创建一个合成层。这使得动画元素在一个独立的层中进行渲染。
当元素的内容没有发生改变，就没有必要进行重绘。浏览器会通过重新复合来创建动画帧

### CSS盒子模型(box-sizing)
CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距（margin）、边框(border)、填充(padding)、内容(content)。  
border-box:给元素设置宽高，此时宽高包括边框、填充、内容三部分  
content-box: 给元素设置宽高，只设置了内容的宽高，不包括边框和填充

### CSS样式优先级
!important>style>id>class

### 什么是BFC？BFC的布局规则是什么？如何创建BFC？BFC应用？
#### 什么是BFC
Formatting Context 是W3C CSS2.1规范中的一个概念。它是页面中的一块渲染区域，并且有一套渲染规则，它决定了其子元素将如何定位，以及和其他元素的关系和相互作用。最常见的Formatting context 有 Block Formatting Context(BFC)和Inline Formatting Context（IFC） 

具有BFC特性的元素可以看作是隔离了的独立容器，容器里面的元素不会在布局上影响到外面的元素，并且BFC具有普通容器所没有的一些特性，通俗一点来讲，可以把
BFC理解为一个封闭的大箱子，箱子内部的元素无论如何翻江倒海，都不会影响到外部。

#### 触发BFC
1.body根元素  
2.浮动元素：float除none以外的值
3.绝对定位元素：position(absolut,fixed)  
4.display为inline-block、table-cells,flex  
5.overflow除了visible以外的值(hidden,auto,scroll)  

#### BFC的特性
__1.同一个BFC下两个元素上下外边距会发生折叠，为避免折叠可以将两个元素放置在两个BFC中__  
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        p {
            width: 100px;
            height: 100px;
            margin: 100px;
            background: lightblue;
        }
    </style>
</head>
<body>
    <p></p>
    <p></p>
</body>
</html>
```
![](https://pic4.zhimg.com/v2-0a9ca8952c83141250a2d9002e6d2047_r.jpg)
从效果上看，因为两个 div 元素都处于同一个 BFC 容器下 (这里指 body 元素) 所以第一个 p 的下边距和第二个 p 的上边距发生了重叠，所以两个盒子之间距离只有 100px，而不是 200px。
首先这不是 CSS 的 bug，我们可以理解为一种规范，__如果想要避免外边距的重叠，可以将其放在不同的 BFC 容器中。__
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        p {
            width: 100px;
            height: 100px;
            margin: 100px;
            background: lightblue;
        }
        div {
            overflow: hidden;
        }
    </style>
</head>
<body>
<div>
    <div class="div">
        <p></p>
    </div>
    <div class="div">
        <p></p>
    </div>
</div>
</body>
</html>
```
![](https://pic2.zhimg.com/v2-5b8d6e8b2b507352900c1ece00018855_r.jpg)

__2.BFC可以包含浮动的元素（清除浮动,解决高度塌陷）__
浮动的元素会脱离普通文档流  
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
      .div1 {
        border: 1px solid #000;
      }
      .div2 {
        width: 100px;
        height: 100px;
        background: #EEEEEE;
        float: left;
      }
    </style>
</head>
<body>
  <div class="div1">
    <div class="div2"></div>
  </div>
</body>
</html>
```
![](https://pic4.zhimg.com/v2-371eb702274af831df909b2c55d6a14b_r.jpg)

由于容器内元素浮动，脱离了文档流，所以容器只剩下2px的边距高度，如果触发容器的BFC，那么容器将会包裹着浮动元素。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
      .div1 {
        border: 1px solid #000;
          overflow: hidden;
      }
      .div2 {
        width: 100px;
        height: 100px;
        background: #EEEEEE;
        float: left;
      }
    </style>
</head>
<body>
  <div class="div1">
    <div class="div2"></div>
  </div>
</body>
</html>
```
![](https://pic4.zhimg.com/80/v2-cc8365db5c9cc5ca003ce9afe88592e7_720w.png)

__3.BFC可以阻止元素被浮动的元素覆盖__  
文字环绕效果
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
  <div style="height: 100px;width: 100px;float: left;background: lightblue">我是一个左浮动的元素</div>
  <div style="width: 200px; height: 200px;background: #eee">我是一个没有设置浮动,
      也没有触发 BFC 元素, width: 200px; height:200px; background: #eee;
  </div>
</body>
</html>
```
![](https://pic4.zhimg.com/80/v2-dd3e636d73682140bf4a781bcd6f576b_720w.png)

这时候其实第二个元素有部分被浮动的元素所覆盖，（但是文本信息不会被浮动元素所覆盖）如果想避免这种情况，可触发
第二个元素的BFC特性，在第二个元素中加入__overflow:hidden__。
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
  <div style="height: 100px;width: 100px;float: left;background: lightblue;">我是一个左浮动的元素</div>
  <div style="width: 200px; height: 200px;background: #eee;overflow: hidden">我是一个没有设置浮动,
      也没有触发 BFC 元素, width: 200px; height:200px; background: #eee;
  </div>
</body>
</html>
```
![](https://pic3.zhimg.com/v2-5ebd48f09fac875f0bd25823c76ba7fa_r.jpg)

### DOM、BOM对象
javascript由ECMAScript、BOM、DOM组成。  

BOM（Browser Object Model）是指浏览器对象模型，可以对浏览器窗口进行访问和操作。使用BOM，开发者可以移动窗口、改变状态栏中的文本以及执行其他与页面内容不直接相关的动作。使用JavaScript有能力与浏览器“对话”。  

DOM（Document Object Model）是指文档对象模型，通过他
可以访问HTML文档的所有元素。DOM是W3C的标准。DOM定义了访问HTML和XML文档的标准。  

ECMAScript是一个标准，JS只是它的一个实现，其他实现包括ActionScript。






