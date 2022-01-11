---
title: basicJs
date: 2022-01-11 10:43:11
tags: 
- js
cover: https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fc-ssl.duitang.com%2Fuploads%2Fitem%2F201807%2F30%2F20180730192202_vixnn.thumb.1000_0.jpeg&refer=http%3A%2F%2Fc-ssl.duitang.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1644461106&t=894d90c7876444bdcb1a483421a8ad49
---

## js基础知识

### js数据类型

基本数值类型：String、Number、Boolean、Null、Undefined、Symbol  
引用数据类型：Object、Array、Function  

null和undefined的区别：null表示空对象，undefined表示已在作用域中声明但未赋值的变量  

typeof主要用来判断数据类型 返回值有string、boolean、number、function、object、undefined  

instanceof 判断该对象是谁的实例

弱类型：在定义变量时，我们可以为变量赋值任何数据，变量的数据类型不是固定死的，这样的类型叫做弱类型  
强类型：在声明变量的时候，一旦给变量赋值，那么变量的数据类型就已经确定，之后如果要给该变量赋值其他类型的数据，需要进行强制类型装换  

动态类型：动态类型的类型检查会在 __代码运行的时候进行__  
静态类型：静态类型的类型检查会在 __编译时进行__

### 作用域和作用域链
在js中，作用域分为全局作用域和局部作用域，全局作用域在程序的任何地方都能访问，局部作用域一般指函数内部的作用域。  
作用域链：函数内部找不到值，就会往上级作用域去找，一直找到全局作用域，这样一个查找过程形成的链条称为作用域链。

### 变量提升
在js编译阶段，会把变量和函数的声明提升至当前作用域的顶端  
注意点：  
1、提升的部分只是变量的声明，赋值语句和可执行代码逻辑还是保持原地不动  
2、提升只是将变量声明和函数声明提升到变量所在的作用域顶端，并不会提示到全局作用域
3、ES6 的let和const声明不存在变量提升

### 闭包
闭包是指有权访问另一个函数作用域中的变量的函数
```javascript
function a() {
    var num = 0;
    function b() {
        num++;
        console.log(num);
    }
    return b;
}

var a1 = a();
a1();
a1();

// 打印结果为1,2
```
正常来说，a方法执行完毕后，a方法的作用域会被垃圾回收机制销毁，但由于此时全局作用域中a1指向了b方法，全局作用域变量不会被回收，故垃圾回收机制认为b还在使用，不会销毁，同时b方法引用了
a方法作用域中的num变量，故num也不会销毁，此时就形成了闭包，每次请求，都在原有的num上++,故打印为1,2  

闭包的优点：  
1、可以读取函数内部的变量  
2、避免全局污染  

闭包的缺点：  
1、闭包会导致变量不会被垃圾回收机制所清除，会大量消耗内存  
2、不恰当的使用闭包可能会造成内存泄漏的问题  

### 原型和原型链
函数的原型：创建（声明）一个函数A，那么浏览器会在内存中创建一个对象B,函数A会有一个默认的属性prototype指向对象B，这个对象B就是函数A的原型对象。这个
原型对象B有个默认属性constructor指向函数A
![](http://lizhenchao.oss-cn-shenzhen.aliyuncs.com/imgs/public/16-11-10/43031030.jpg)

将函数A作为构造函数使用new创建对象C，对象C就会存在一个属性__proto__([[Prototype]])指向原型对象B
![](http://lizhenchao.oss-cn-shenzhen.aliyuncs.com/imgs/public/16-11-10/6663492.jpg)

__Person函数举例__  
```javascript
function Person()
{

}

var person = new Person();
person.name = 'Tian';
console.log(person.name);    //Tian
```
Person有个属性prototype指向原型对象(这里称为对象A，Person.prototype)，原型对象有个属性constructor指向Person,实例person有个属性__proto__指向原型对象
![](https://s4.ax1x.com/2022/01/11/7mAFOg.png)
原型对象也是对象，构造函数为Object,构造函数Object的prototype指向原型对象B(Object.prototype)，原型对象B的constructor属性指向构造函数Object,原型对象A的__proto__指向原型对象B原型对象B(Object.prototype)
![](https://s4.ax1x.com/2022/01/11/7mEP41.png)

原型链：实例的属性会先从实例的构造函数找，当前实例没有就会往构造函数的原型上找，直到找到构造函数Object的原型对象为止，Object.prototype.__proto__指向null,这样一个找寻过程形成的链路叫做原型链
![](https://s4.ax1x.com/2022/01/11/7mEjPI.png)
