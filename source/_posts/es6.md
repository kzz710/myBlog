---
title: es6
date: 2022-01-13 16:27:11
tags:
- es6
- js
cover: https://s4.ax1x.com/2022/01/13/7Qj0kq.png
---

## es6基本知识

### 一、let/const
1、const/let都是用来声明变量，不可重复声明，具有块级作用域。存在暂时性死区，不存在变量提升。const用来声明常量。  

### 二、symbol
1、symbol是一个全新的数据类型，表示独一无二的值，对象可以以symbol类型作为键  
2、现在主要用于声明对象的私有属性  
__示例__
```javascript
console.log(Symbol() === Symbol()); // false
console.log(Symbol('foo') === Symbol('foo')); // false
console.log(typeof Symbol()); // symbol
console.log(Symbol('foo')); // Symbol('foo')

const obj = {
    [Symbol('name')]: '张三'
}
console.log(obj) // {[Symbol(name)]:'张三'}
console.log(obj[Symbol('name')]) // undefined

let name = Symbol('name');
const Person = {
    [name]: 'zhangsan',
    age: 15,
    say() {
        console.log('i am ' + this[name])
    }
}

console.log(Person[Symbol('name')]) // undefined 故symbol一般用于定义私有属性 ，外部访问不到
Person.say() // i am zhangsan

console.log(Symbol.for('true') === Symbol.for(true)) // true  Symbol.for方法是根据字符串参数作比较是否相等

const objj = {
    [Symbol('name')]: 'lisi',
    age: 15
}

for (let k in objj) {
    console.log(k); // age  for方法无法访问到Symbol属性
}

console.log(Object.keys(objj)) // ['age']  Object.keys也无法访问Symbol属性

console.log(JSON.stringify(objj)) // {'age':15}

console.log(Object.getOwnPropertySymbols(objj)) // [Symbol(name)] 能获取到Symbol属性

console.log(Reflect.ownKeys(objj)) // ['age', Symbol(name)] 能获取到Symbol属性
```

### 三、对象和数组的解构
__示例__  
1、数组的解构
```javascript
// 传统方法
const arr = [1, 2, 3];
let fir = arr[0];
let sec = arr[1];
let thr = arr[2];
console.log(fir, sec, thr) // 1,2,3

//es6新增解构方法
let [f, s, t] = arr;
console.log(f,s,t) // 1,2,3

let [f, ...rest] = arr;
console.log(f, rest) // 1, [2, 3]

let [f, a, b, c, d] = arr;
console.log(f,a,b,c,d) // 1,2,3,undefined，undefined

let [,,a,b,c] = arr;
console.log(a,b,c) // 3 undefined undefined
```
2、对象的解构
```javascript
const obj = {
    name: 'zhangsan',
    age: 15
}

const {name, age} = obj;
console.log(name, age); // zhangsan 15

const name1 = 'lisi';

const {name: name2, age, sex='man'} = obj;

console.log(name1, name2, age, sex); // lisi zhangsan 15 man
```

### 四、模板字符串
__示例__  
```javascript
let str = `我是模板字符串 我能
直接换行`
console.log(str);
//我是模板字符串 我能
//直接换行

const {name, age, sex} = {
    name: 'zhangsan',
    age: 18,
    sex: 'man'
}

let s = `大家好，我是${name}, 今年${age}, ${1+20}，${Math.random()}`
console.log(s) // 大家好，我是zhangsan, 今年18, 21，0.45371632552390184

function tag(str, sex, name, age) {
    console.log(str, sex, name, age)
}
//tag方法使用于模板字符串，第一个参数返回以${}拆分的数组，剩余的参数与目标字符穿的顺序一一对应
const a = tag`大家好，我是${sex}，姓名：${name}, 年龄${age}, 哈哈`; // [ '大家好， 我是', '，姓名：', ', 年龄', ',哈哈' ] 'man' 'zhangsan' 18
console.log(a); // undefined
```

### 五、扩展运算符(...)  
对象中的扩展运算符(...)用于取出参数对象中的所有可遍历属性，拷贝到当前对象之中,属于浅拷贝  
__示例__  
```javascript
const a = {
    name: 'zhangsan',
    age: 18,
    friends: ['xm', 'xh']
}
const b = {
    name: 'lisi',
    sex: 'woman'
}

const c = {
    ...a,
    ...b
}
console.log(c)  // { name: 'lisi', age: 18, friends: [ 'xm', 'xh' ], sex: 'woman' }

// c引用数据类型发生改变，a也会随之改变，证明扩展运算符是浅拷贝
c.friends[0] = 'xt';
c.age = 20;
console.log(a) // { name: 'zhangsan', age: 18, friends: [ 'xt', 'xh' ] }
console.log(c) // { name: 'lisi', age: 20, friends: [ 'xt', 'xh' ], sex: 'woman' }
```

### 六、字符串新增方法
1、includes、startsWith、endsWith
```javascript
let s = 'hello es6';
console.log(s.includes('es')); // true  判断字符串是否包含某字符串
console.log(s.startsWith('e')); // false 判断字符串是否以某字符串开头
console.log(s.endsWith('6')); // true  判断字符串是否以某字符串结尾
```

2、repeat方法
```javascript
let i = 'abc';
console.log(i.repeat(3)) // abcabcabc 表示重复某字符串几次
```

3、padStart、padEnd方法
```javascript
let x = 'x';
console.log(x.padStart(5, 'ab')); // ababx  在x字符串前面，用ab补全，总共5位  第一个参数补全之后的位数，第二个参数补全用的字符
console.log(x.padStart(4, 'ab')); // abax   在x字符串前面，用ab补全，总共4位  第一个参数补全之后的位数，第二个参数补全用的字符
console.log(x.padEnd(5, 'ab')); // xabab    在x字符串后面，用ab补全，总共5位  第一个参数补全之后的位数，第二个参数补全用的字符
console.log(x.padEnd(4, 'ab')); // xaba     在x字符串后面，用ab补全，总共4位  第一个参数补全之后的位数，第二个参数补全用的字符
```

4、replaceAll方法
```javascript
let o = 'abbcc';
let o1 = o.replaceAll('b', '_');
console.log(o) // 'abbcc'
console.log(o1) // 'a__cc'
```

### 七、箭头函数
__1、箭头函数中this是在定义是绑定的，而不是调用时。（箭头函数的this值继承自外围作用域。运行时它会首先到它的父级作用域找，如果父级作用域还是箭头函数，那么接着向上找，直到找到我们要的this指向）__
```javascript
var a1 = 'ppp'

var obj1 = {
    a1: 'bbb',
    b2: this.a1,
    f: function () {
        console.log(this.a1)
    },
    d: () => {
        console.log(this.a1)
    }
}
console.log(obj1.b2) // ‘ppp’
obj1.f(); // ‘bbb’
obj1.d(); // 'ppp'
```
__2、箭头函数不能作为构造函数，不能使用new__  
```javascript
//构造函数如下：
function Person(p){
    this.name = p.name;
}
//如果用箭头函数作为构造函数，则如下
var Person = (p) => {
    this.name = p.name;
}
```
由于this必须是实例对象，而箭头函数是没有实例的，此处的this指向window，不能产生person的实例，自相矛盾  

__3、箭头函数没有arguments、caller、callee__  
箭头函数本身没有arguments,如果箭头函数在一个function内部，它会将外部函数的arguments拿过来使用。  
箭头函数中要想接手不定参数，应该使用rest参数...解决
```javascript
let B = (b)=>{
  console.log(arguments);
}
B(2,92,32,32);   // Uncaught ReferenceError: arguments is not defined

let C = (...c) => {
  console.log(c);
}
C(3,82,32,11323);  // [3, 82, 32, 11323]
```

__4、箭头函数通过call和apply调用，不会改变this指向，只会传入参数__  
```javascript
var name = 'lw'
let a1 = {
    name: 'zs',
    f: function (age) {
        console.log(`我是${this.name},年龄${age}`)
    },
    d: (age) => {
        console.log(`我是${this.name},年龄${age}`)
    }
}

let a2 = {
    name: 'ls'
}

a1.f(15); // 我是zs,年龄15
a1.d(15); // 我是lw,年龄15
a1.f.call(a2, 18); // 我是ls,年龄18
a1.d.call(a2, 18); // 我是lw,年龄18
```

__5、箭头函数没有原型属性__  
```javascript
var A = function () {
    return 2
}

var B = () => {
    return 3
}

console.log(A.prototype) // {constructor: ƒ}
console.log(B.prototype) // undefined
```

__6、箭头函数ES6 class中声明的方法为实例方法，不是原型方法__  
```javascript
class Super {
    sayHello() {
        console.log('hello')
    }
    sayWorld = () => {
        console.log('world')
    }
}
const a = new Super();
const b = new Super();

console.log(a.sayHello === b.sayHello) // true sayHello是Super.prototype上的方法，所有实例共享同一个方法，所以为true
console.log(a.sayWorld === b.sayWorld) // false sayWorld是各自实例上的方法，所以每个方法不一样，估为false
console.log(Super.prototype)

```


