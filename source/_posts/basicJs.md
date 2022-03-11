---
title: basicJs
date: 2022-01-11 10:43:11
tags: 
- js
cover: https://s4.ax1x.com/2022/01/13/7QjJ1S.png
---

## js基础知识

### 一、js数据类型

基本数值类型：String、Number、Boolean、Null、Undefined、Symbol  
引用数据类型：Object、Array、Function  

null和undefined的区别：null表示空对象，undefined表示已在作用域中声明但未赋值的变量  

typeof主要用来判断数据类型 返回值有string、boolean、number、function、object、undefined  

instanceof 判断该对象是谁的实例

弱类型：在定义变量时，我们可以为变量赋值任何数据，变量的数据类型不是固定死的，这样的类型叫做弱类型  
强类型：在声明变量的时候，一旦给变量赋值，那么变量的数据类型就已经确定，之后如果要给该变量赋值其他类型的数据，需要进行强制类型装换  

强类型不允许任意类型的隐式类型转换，而弱类型是允许的

动态类型：动态类型的类型检查会在 __代码运行的时候进行__  
静态类型：静态类型的类型检查会在 __编译时进行__

### 二、作用域和作用域链
在js中，作用域分为全局作用域和局部作用域，全局作用域在程序的任何地方都能访问，局部作用域一般指函数内部的作用域。  
作用域链：函数内部找不到值，就会往上级作用域去找，一直找到全局作用域，这样一个查找过程形成的链条称为作用域链。

### 三、变量提升
在js编译阶段，会把变量和函数的声明提升至当前作用域的顶端  
注意点：  
1、提升的部分只是变量的声明，赋值语句和可执行代码逻辑还是保持原地不动  
2、提升只是将变量声明和函数声明提升到变量所在的作用域顶端，并不会提示到全局作用域
3、ES6 的let和const声明不存在变量提升
4、函数是一级公民，优先提升

### 四、闭包
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

### 五、原型和原型链
函数的原型：创建（声明）一个函数A，那么浏览器会在内存中创建一个对象B,函数A会有一个默认的属性prototype指向对象B，这个对象B就是函数A的原型对象。这个原型对象B有个默认属性constructor指向函数A
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
原型对象也是对象，构造函数为Object,构造函数Object的prototype指向原型对象B(Object.prototype)，原型对象B的constructor属性指向构造函数Object,原型对象A的__proto__指向原型对象B(Object.prototype)
![](https://s4.ax1x.com/2022/01/11/7mEP41.png)

原型链：实例的属性会先从实例的构造函数找，当前实例没有就会往构造函数的原型上找，直到找到构造函数Object的原型对象为止，Object.prototype.__proto__指向null,这样一个找寻过程形成的链路叫做原型链
![](https://s4.ax1x.com/2022/01/11/7mEjPI.png)

### 六、this指向
this永远指向最后调用它的对象  
例1
```javascript
var name = '小王', age = 17;
var obj = {
    name: '小张',
    objAge: this.age,
    myFun: function () {
        console.log(this.name + "年龄" + this.age);
    }
}
console.log(obj.objAge) // 17
obj.myFun() // 小张年龄undefined
let f = obj.myFun;
f();  // 小王年龄17
```
例2
```javascript
var fav = '盲僧';
function shows() {
    console.log(this.fav);
}
shows()// 盲僧
```
this永远指向最后调用它的方法的对象，示例1  obj.myFun()this指向obj,将obj.myFun赋值给f,f调用，此时f()等同于window.fn(),故this指向window对象，示例2同理  

函数对象的call()、apply()、bind()能改变this指向  

__call、apply、bind方法详解__  
例1  
```javascript
var name = '小王', age = 17;
    var obj = {
        name: '小张',
        objAge: this.age,
        myFun: function () {
            console.log(this.name + "年龄" + this.age);
        }
    }
    var db = {
        name: '德玛',
        age: 99
    }

    obj.myFun.call(db) // 德玛年龄99
    obj.myFun.apply(db) // 德玛年龄99
    obj.myFun.bind(db)() // 德玛年龄99
```
call、apply、bind都能改变this指向，bind方法返回是一个方法，需要再次调用  

例2
```javascript
var name = '小王', age = 17;
    var obj = {
        name: '小张',
        objAge: this.age,
        myFun: function (fm, to) {
            console.log(this.name + "年龄" + this.age, "来自" + fm + "去往" + to);
        }
    }
    var db = {
        name: '德玛',
        age: 99
    }

    obj.myFun.call(db, '上海', '成都') // 德玛年龄99 来自上海去往成都
    obj.myFun.apply(db, ['上海', '成都']) // 德玛年龄99 来自上海去往成都
    obj.myFun.bind(db, '上海', '成都')() // 德玛年龄99 来自上海去往成都
    obj.myFun.bind(db, ['上海', '成都'])() // 德玛年龄99 来自上海,成都去往undefined
```
call、apply、bind第一个参数都是this指向的对象，第二个参数，call是直接放入，apply是放入数组，bind和call参数形式一样，除了需要再次调用  

### 七、new关键字
是用new一个构造函数创建对象实例的过程  
```javascript
function Pro(){
    this.x = '1';
    this.y = function(){};
}
var p = new Pro();
```
1、创建一个空对象  
2、将空对象的__proto__指向构造函数的原型对象（Pro.prototype）  
3、将构造函数中的this指向此对象(new关键字会自动调用函数的apply方法，将this指向这个空对象吗)
4、执行构造函数代码为对象赋值  
5、返回本对象(判断构造函数的返回值，如果是值类型，则返回创建的对象，如果是引用类型则返回引用类型对象本身)  
![](https://pic2.zhimg.com/v2-7ce5f71bd0865872b513a88fabb597fd_r.jpg)

__补充知识点__  
1、在严格模式中默认的this不再是window，而是undefined

### 八、继承
父类准备
```javascript
// 定义一个动物类
function Animal(name) {
  // 属性
  this.name = name || 'Animal';
  // 实例方法
  this.sleep = function () {
    console.log(this.name + '正在睡觉');
  }
}
// 原型方法
Animal.prototype.eat = function (food) {
  console.log(this.name + "正在吃" + food);
}
```
__1、原型链继承__  
核心：将父类的实例作为子类的原型
```javascript
function Cat(name) {
      this.name = name;
}

Cat.prototype = new Animal();
Cat.prototype.age = 1;

var cat = new Cat('ximi');
console.log(cat.name, cat.age); // ximi 1
cat.eat('fish'); // ximi正在吃fish
cat.sleep(); // ximi正在睡觉
console.log(cat instanceof Cat); // true
console.log(cat instanceof Animal); // true
```
特点：  
1、非常纯粹的继承关系，实例是子类的实例，也是父类的实例  
2、父类新增原型方法/原型属性，子类都能访问到  
3、简单，易于实现  
缺点：  
1、要新增子类原型属性和方法，必须放在new Animal()这样的语句之后执行  
2、无法实现多继承  
3、来自原型对象的所有属性被所有实例共享  
4、创建子类实例时，无法向父类构造函数传参  

__2、构造继承__  
核心：使用父类的构造函数来增强子类实例，等于是复制父类的实例属性给子类（没用到原型）  
```javascript
function Cat(name) {
    Animal.call(this, 'ximi');
    this.name = name || 'putao';
}

var cat = new Cat()
console.log(cat.name) // putao
cat.eat('fish'); // 报错
cat.sleep(); // ximi正在睡觉
console.log(cat instanceof Cat) // true
console.log(cat instanceof Animal) // false
```
特点：  
1、只继承了父类构造函数的属性和方法，没有继承父类原型的属性和方法  
2、可以继承多个构造函数属性和方法（call多个）  
3、创建子类实例时可以向父类传参
缺点：  
1、实例并不是父类的实例，只是子类的实例  
2、只能继承父类的属性和方法，不能继承父类原型的属性和方法  
3、无法实现函数复用，每个子类都有父类实例函数的副本，影响性能  

__3、实例继承__  
核心：为父类实例添加新特性，作为子类实例返回  
```javascript
function Cat(name) {
    var instance = new Animal('ximi');
    instance.name = name || 'putaa';
    return instance
}

var cat = new Cat('ppt');
console.log(cat.name); // ppt
cat.eat('鱼'); //ppt正在吃鱼
cat.sleep(); //ppt正在睡觉
console.log(cat instanceof Cat); //false
console.log(cat instanceof Animal); // true
```
特点：  
1、不限制调用方式，不管是new 子类()还是子类()，返回的对象具有相同的效果  
缺点：  
1、实例是父类的实例，不是子类的实例  
2、不支持多继承  

__4、拷贝继承__  
```javascript
function Cat(name) {
    var instance = new Animal(name);

    for (let key in instance) {
        this[key] = instance[key];
    }
}

var cat = new Cat('bbt');
console.log(cat.name); //bbt
cat.eat('肉'); // bbt正在吃肉
cat.sleep(); // bbt正在睡觉
console.log(cat instanceof Cat); // true
console.log(cat instanceof Animal); // false
```
特点：  
1、支持多继承  
缺点：  
1、效率低，内存占用高，因为要拷贝父类的属性  
2、无法获取父类不可枚举的方法  

__5、组合继承__  
核心：通过调用父类构造函数，继承父类属性并保留传参的特点，然后将通过将父类的实例作为子类的原型，实现函数复用
```javascript
function Cat(name) {
    Animal.call(this, name);
}
Cat.prototype = new Animal();
var cat = new Cat('ttp');
console.log(cat.name); // ttp
cat.eat('白菜'); // ttp正在吃白菜
cat.sleep(); // ttp正在睡觉
console.log(cat instanceof Cat); // true
console.log(cat instanceof Animal); // true
```
特点：  
1、弥补了方式2的缺陷，可以继承实例的属性和方法，也能继承原型的属性和方法  
2、既是子类的实例，也是父类的实例  
3、不存在引用属性共享的问题  
4、可传参  
5、函数可复用  
缺点：  
1、调用两次父类的构造函数，生成两份实例（子类实例将子类原型上的那份屏蔽了）  

__6、寄生组合继承__  
核心：通过寄生方式，砍掉父类的实例属性，这样，在调用两次父类的构造的时候，就不会初始化两次方法和属性，避免组合继承的缺陷  
```javascript
function Cat(name) {
    Animal.call(this,name);
}

var Super = function () {}

Super.prototype = Animal.prototype

Cat.prototype = new Super();

Cat.prototype.constructor = Cat;

var cat = new Cat('pig');

console.log(cat.name); // pig
cat.eat('白菜'); // pig正在吃白菜
cat.sleep(); // pig正在睡觉
console.log(cat instanceof Cat); // true
console.log(cat instanceof Animal); // true
```
特点：  
1、解决了两次调用父类的构造函数  
缺点：  
1、实现较为复杂

### 九、EventLoop (事件循环)
JS是单线程的，为了防止一个函数的执行事件过长阻塞后面的代码，所以会先把同步代码压入执行栈中，依次执行，将异步代码推入异步队列，当执行栈中没有执行任务时，EventLoop开始工作，将异步队列中的任务依次压入执行栈中执行，直到所有异步队列执行完毕。
![](https://s4.ax1x.com/2022/01/13/7QcOG6.png)
![](https://s4.ax1x.com/2022/01/13/7Qgliq.png)
注意：  
1、js是单线程的，但是浏览器是多线程的，执行异步任务是异步线程执行  
2、异步任务也分为宏任务和微任务，微任务优先宏任务执行，微任务队列的代表：Promise.then,MutationObserver。宏任务的代表：setTimeout,setInterval,setImmediate  

### 十、事件冒泡、捕获、委托
DOM事件流：事件捕获、处于目标阶段、事件冒泡  
事件委托：就是利用事件冒泡机制，将事件绑定在目标DOM的父级上，触发执行效果  
事件委托的优势：1、减少DOM操作，提高性能。2、随时可以添加子元素，添加的子元素会自动有相应的处理事件  

### 十一、原生AJAX
```javascript
var Ajax = {
    get: function (url, callback) {
        // XMLHttpRequest对象用于在后台与服务器交换数据
        var xhr = new XMLHttpRequest();
        //参数说明：方法名GET/POST   请求地址   async 请求进行异步还是同步，true服务器响应时执行其他脚本，false是等待服务器响应再执行
        xhr.open('GET', url, false);
        xhr.onreadystatechange = function () {
            // readyState == 4说明请求已完成
            if (xhr.readyState == 4 && xhr.status == 200 || xhr.status == 304) {
                // 从服务器获得数据
                callback(xhr.responseText)
            }
        }
        xhr.send();
    },
    post: function (url, data, callback) {
        var xhr = new XMLHttpRequest();
        xhr.open('POST',url, false);
        xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded');
        xhr.onreadystatechange = function () {
            if (xhr.readyState == 4) {
                if (xhr.status == 200 || xhr.status == 304) {
                    callback(xhr.responseText);
                }
            }
        }
        xhr.send(data);
    }
}
```

### 十二、js深拷贝和浅拷贝
如何区分浅拷贝和深拷贝，简单来说，假设B复制A，当修改A的引用数据时，看B是否发生改变，如果B跟着变了，就是浅拷贝，如果没变就是深拷贝  
浅拷贝的方式：  
1、直接=赋值  
2、for in  
3、Object.assign()  

深拷贝的方式：  
1、采用递归去拷贝所有层级的属性
```javascript
function deepClone(obj) {
    let objClone = Array.isArray(obj) ? []: {}
    if (typeof obj === 'object') {
        Object.keys(obj).forEach(key => {
            if (obj[key] && typeof obj[key] === 'object') {
                objClone[key] = deepClone(obj[key])
            } else {
                objClone[key] = obj[key]
            }
        })
    } else {
        objClone = obj
    }
    return objClone;
}

let a = {
    name: 'zs',
    age: 18,
    son: {
        name: 'ls',
        age: 1
    }
}
let b = deepClone(a) //深拷贝
let c = Object.assign({}, a); //浅拷贝
let d = a; //浅拷贝
let e = {};
for (let key in a) { //浅拷贝
    e[key] = a[key]
}
a.son.name = 'zs';
console.log(a); // { name: 'zs', age: 18, son: { name: 'zs', age: 1 } }
console.log(b); // { name: 'zs', age: 18, son: { name: 'ls', age: 1 } }
console.log(c); // { name: 'zs', age: 18, son: { name: 'zs', age: 1 } }
console.log(d); // { name: 'zs', age: 18, son: { name: 'zs', age: 1 } }
console.log(e); // { name: 'zs', age: 18, son: { name: 'zs', age: 1 } }
```

2、才用JSON的方法
```javascript
let a = {
    name: 'zs',
    age: 18,
    son: {
        name: 'ls',
        age: 1
    }
}
let b = JSON.parse(JSON.stringify(a));
a.son.name = 'ww';
console.log(a); // { name: 'zs', age: 18, son: { name: 'ww', age: 1 } }
console.log(b); // { name: 'zs', age: 18, son: { name: 'ls', age: 1 } }

//缺点：无法复制方法属性

let a1 = {
    name: 'zs',
    age: 18,
    son: {
        name: 'ls',
        age: 1
    },
    say() {
        console.log('hello')
    }
}
let b1 = JSON.parse(JSON.stringify(a1));
a1.son.name = 'ww';
console.log(a1); // { name: 'zs',age: 18,son: { name: 'ww', age: 1 },say: [Function: say] }
console.log(b1); // { name: 'zs', age: 18, son: { name: 'ls', age: 1 } }
```

3、利用jQuery的extend方法
```javascript
let arr = [1,2,3,4]
let newArr = $.extend(true, [], arr) // true为深拷贝，false为浅拷贝
```

4、loadsh函数库实现
```javascript
let obj = {
    name: 'z1',
    son: {
        name: 'zs'
    }
}
let result = _.cloneDeep(obj);
```

