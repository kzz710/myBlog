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

### 八、Proxy代理
```javascript
const person = {
    name: 'zs',
    age: 18
}

const personProxy = new Proxy(person, {
    get(target, key, receiver) {
        console.log(target, key, receiver)
        return key in target ? target[key] : 'default';
    },
    set(target, key, value, receiver) {
        console.log(target, key, value, receiver)
        if (typeof value !== 'number') {
            throw new TypeError('age need number');
        }
        target[key] = value;
    }
})

console.log(personProxy.name) // {name: 'zs', age: 18}  'name'  Proxy{name: 'zs', age: 18}  zs
console.log(personProxy.sex) // {name: 'zs', age: 18}  'sex'  Proxy{name: 'zs', age: 18}  default
console.log(personProxy.age) // {name: 'zs', age: 18}  'age'  Proxy{name: 'zs', age: 18}  18
personProxy.age = 19  // // {name: 'zs', age: 18}  'age'  19  Proxy{name: 'zs', age: 18}  
personProxy.age = '20';  // {name: 'zs', age: 19}  'age'  '20'  Proxy{name: 'zs', age: 19}  报错Uncaught TypeError: age need number


const list = [1,2,3]
const listProxy = new Proxy(list, {
    get(target, key, receiver) {
        console.log('get')
        return target[key]
    },
    set(target, key, value, receiver) {
        console.log('set')
        target[key] = value;
        return true; // 表示设置成功
    },
    deleteProperty(target, key) {
        console.log('delete')
    }
})
console.log(listProxy[0]);
console.log(listProxy.push(100));
console.log(listProxy.shift())
listProxy[0] = 999
```
Proxy代理 vs Object.defineProperty()  
1、Object.defineProperty()只能监听对象的读写操作  而Proxy能监听更多的操作，比如deleteProperty、has等  
2、Proxy能监听数组的操作  
3、Proxy是以非侵入式的监管对象，不会对原对象进行任何的操作

### 八、Reflect
Reflect提供了对象统一操作API
```javascript
const person = {
    name: 'zs',
    age: 19
}
function Person() {
    this.name = 'ls';
    this.age = 20;
}
Person.prototype.say = function () {
    console.log('hello world')
}
const p = new Person();
console.log(Reflect.get(person, 'name'))  // zs
console.log(Reflect.ownKeys(person)) // ['name', 'age']  只能访问对象上的属性，不能访问原型上的
console.log(Reflect.ownKeys(p)) // ['name', 'age']
console.log(Reflect.set(person, 'age', 20)) // true 表示操作成功
console.log(Reflect.has(person, 'sex')) // false
console.log(Reflect.deleteProperty(person, 'age')) // true 表示操作成功
```

### 九、set和map
1、set数据结构 返回一个没有重复值的集合  
```javascript
const s = new Set()
s.add('a').add('b').add(3).add(4).add(5).add(5);

s.forEach(i => console.log(i))  // a b 3 4 5

for (let i of s) {
    console.log(i) // a b 3 4 5
}

console.log(s) // Set(5){'a', 'b', 3, 4, 5}
console.log(s.size) // 5
console.log(s.has('a')) // true
console.log(s.has(100)) // false
console.log(s.delete(5)) // true 表示操作成功 删除5
console.log(s) // Set(4){'a', 'b', 3, 4}
console.log(s.clear()) // undefined
console.log(s) // Set(0){size: 0}

// 数组去重
const array = [1,3,2,3,1,5,6,1,5];
const newArr = [...new Set(array)]
console.log(newArr) // [1, 3, 2, 5, 6]
```
2、map数据结构 键值对数据结构
```javascript
// 传统键值对象，会将key不是字符串的值转变为字符串
const obj = {};
obj[123] = 'value';
obj[true] = 'value';
obj[{name:'zs'}] = 'value';
obj['zzh'] = 'value';
console.log(Reflect.ownKeys(obj)) // ['123', 'true', '[object Object]', 'zzh']

// map对象，会将key的数据类型保留
const t = {
    name: 'kzz'
}
const m = new Map();
m.set(t, 'a');
m.set(true, 'b');
m.set(123, 'c');
console.log(m); // {{…} => 'a', true => 'b', 123 => 'c'}
console.log(m.get(t)) // a
console.log(m.get(true)) // b
m.forEach((i, k) => console.log(i, k)) // a {name: 'kzz'}  b true  c 123
```

### 十、class
1、ES6的类，完全可以看成构造函数的另外一种写法  
```javascript
class Point {
  // ...
}

typeof Point // "function"
Point === Point.prototype.constructor  // true
```
2、类的方法都是定义在prototype对象上（箭头函数方法是定义在实例上）  
3、类的内部定义的方法，都是不可枚举的,这和ES5的行为不一致
```javascript
class Point {
  constructor(x, y) {
    // ...
  }

  toString() {
    // ...
  }
}

Object.keys(Point.prototype)
// []
Object.getOwnPropertyNames(Point.prototype)
// ["constructor","toString"]
```
4、constructor方法是类的默认方法，通过new对象创建实例时，会自动调用该方法。一个类必定有constructor方法，如果没有显式定义，一个空的constructor方法会默认被添加  
```javascript
class Point {
}

// 等同于
class Point {
  constructor() {}
}
```
5、constructor方法默认返回实例对象（this）,完全可以指定返回另外一个对象
```javascript
class Foo {
  constructor() {
    return Object.create(null);
  }
}

new Foo() instanceof Foo
// false
// constructor返回一个全新的对象，结果导致实例对象不是Foo的实例
```
6、静态方法  
类相当于实例的原型，所有在类中定义的方法，都会被实例继承，如果在一个方法前加上static关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为静态方法
```javascript
class Foo {
  static classMethod() {
    return 'hello';
  }
}

Foo.classMethod() // 'hello'

var foo = new Foo();
foo.classMethod()
// TypeError: foo.classMethod is not a function
```
如果静态方法中包含关键字this,这个this指向的是类而不是实例
```javascript
class Foo {
  static bar() {
    this.baz();
  }
  static baz() {
    console.log('hello');
  }
  baz() {
    console.log('world');
  }
}

Foo.bar() // hello
```
父类的静态方法能被子类继承
```javascript
class Foo {
  static classMethod() {
    return 'hello';
  }
}

class Bar extends Foo {
}

Bar.classMethod() // 'hello'
```

7、静态属性  
使用static 声明的属性就是静态属性，由类调用

### 十一、类的继承
1、class可以通过extends关键字实现继承
```javascript
class Point {
}

class ColorPoint extends Point {
  constructor(x, y, color) {
    super(x, y); // 调用父类的constructor(x, y)
    this.color = color;
  }

  toString() {
    return this.color + ' ' + super.toString(); // 调用父类的toString()
  }
}
```
上述代码中，constructor和toString方法之后，都出现了super关键字，它在这里表示父类的构造函数，用来新建父类的实例  
子类必须在constructor方法中调用super方法，否则新建实例时会报错。这是因为子类自己的this对象，必须先通过父类的构造函数完成塑造，得到与父类同样的实例属性和方法，然后再对其进行加工，加上子类自己的实例属性和方法。如果不调用super方法，子类就得不到this对象。  
```javascript
class Point { /* ... */ }

class ColorPoint extends Point {
  constructor() {
  }
}

let cp = new ColorPoint(); //ReferenceError
```
ES6的继承机制实质上是将父类的实例对象的属性和方法，加在this上面，所以必须先调用super方法，然后再用子类的构造函数修改this  
如果子类没有定义constructor方法，这个方法会被默认添加
```javascript
class ColorPoint extends Point {
}

// 等同于
class ColorPoint extends Point {
  constructor(...args) {
    super(...args);
  }
}
```
在子类的constructor方法没有调用super之前，就使用this关键字，结果报错
```javascript
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }
}

class ColorPoint extends Point {
  constructor(x, y, color) {
    this.color = color; // ReferenceError
    super(x, y);
    this.color = color; // 正确
  }
}
```

2、super关键字
super关键字既可以当做函数使用，也可以当做对象使用  

第一种情况，super作为构造函数使用，代表父类的构造函数，ES6要求，子类的构造函数必须执行一次super函数
```javascript
class A {}

class B extends A {
  constructor() {
    super();
  }
}
```
注意，super虽然代表了父类的构造函数，但是返回是子类B的实例，即super内部的this指向的是B的实例，因此super()相当于A.prototype.constructor.call(this)。  

第二种情况，super作为对象，在普通方法中指向父类的原型对象，在静态方法中，指向父类  
```javascript
class A {
  p() {
    return 2;
  }
}

class B extends A {
  constructor() {
    super();
    console.log(super.p()); // 2
  }
}

let b = new B();
```
上面代码中，子类B当中的super.p()，就是将super当做一个对象。这是super在普通方法中，指向A.prototype，所以super.p()相当于A.prototype.p()。  

注意：这里的super指向的是父类的原型对象，所以定义在父类实例上的方法和属性，是无法通过super调用的
```javascript
class A {
  constructor() {
    this.p = 2;
  }
}

class B extends A {
  get m() {
    return super.p;
  }
}

let b = new B();
b.m // undefined
```

ES6规定，在子类的普通方法中通过super调用父类的方法时，方法内部this指向当前子类的实例  
```javascript
class A {
  constructor() {
    this.x = 1;
  }
  print() {
    console.log(this.x);
  }
}

class B extends A {
  constructor() {
    super();
    this.x = 2;
  }
  m() {
    super.print();
  }
}

let b = new B();
b.m() // 2
```
上面代码中，super.print()虽然调用的是A.prototype.print()，但是A.prototype.print()内部的this指向子类B的实例，导致输出的是2，而不是1。也就是说，实际上执行的是super.print.call(this)。  

由于this指向子类实例，所以如果通过super对某个属性赋值，这时super就是this，赋值的属性会变成子类实例的属性。  
```javascript
class A {
  constructor() {
    this.x = 1;
  }
}

class B extends A {
  constructor() {
    super();
    this.x = 2;
    super.x = 3;
    console.log(super.x); // undefined
    console.log(this.x); // 3
  }
}

let b = new B();
```
上面代码中，super.x赋值为3，这时等同于对this.x赋值为3。而当读取super.x的时候，读的是A.prototype.x，所以返回undefined。

如果super作为对象，用在静态方法之中，这时super将指向父类，而不是父类的原型对象。
```javascript
class Parent {
  static myMethod(msg) {
    console.log('static', msg);
  }

  myMethod(msg) {
    console.log('instance', msg);
  }
}

class Child extends Parent {
  static myMethod(msg) {
    super.myMethod(msg);
  }

  myMethod(msg) {
    super.myMethod(msg);
  }
}

Child.myMethod(1); // static 1

var child = new Child();
child.myMethod(2); // instance 2
```

在子类的静态方法中通过super调用父类的方法时，方法内部的this指向当前的子类，而不是子类的实例。  
```javascript
class A {
  constructor() {
    this.x = 1;
  }
  static print() {
    console.log(this.x);
  }
}

class B extends A {
  constructor() {
    super();
    this.x = 2;
  }
  static m() {
    super.print();
  }
}

B.x = 3;
B.m() // 3
```