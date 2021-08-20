---
title: vue响应式原理剖析及模拟实现
date: 2021-08-17 18:39:37
tags:
 - vue
 - 双向绑定
 - 观察者模式
cover: http://i0.hdslb.com/bfs/article/62ab79fcd20d1368b8039a78f16adbaa1e9b8d35.jpg
---
## vue响应式原理分析
![](https://img.cmvideo.cn/publish/noms/2021/08/18/1O32J6GFE0LJH.png)
1、初始化Vue实例时，将初始化传入data对象属性替换成getter和setter，并注入Vue的实例。  
2、通过Observer对象对data数据进行数据劫持，即将data所有属性通过Object的defineProperty方法都添加get和set方法，这样每当数据获取和改变就会触发get和set方法，从而做到数据监听。  
3、通过compiler对象解析模板和指令，即差值表达式和v-指令。  
4、在Observer给data对象注入get和set方法的同时，给每个属性都添加一个订阅者Dep对象。  
5、在每个用到data属性的地方都添加一个观察者Watcher，每当数据发送变化，订阅者Dep通知观察者Watcher更新视图

## vue代码模拟实现
### 1、项目结构
![](https://i.loli.net/2021/08/19/3XuCZKE92zvTix5.png)
#### myVue.html代码
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <div id="app">
        <div>差值表达式</div>
        <div>{{name}}</div>
        <div>{{age}}</div>
        <div>v-text</div>
        <div v-text="name"></div>
        <div v-text="age"></div>
        <div>v-html</div>
        <div v-html="name"></div>
        <div v-html="age"></div>
        <div>v-model</div>
        <input type="text" v-model="name">
        <input type="text" v-model="age">
    </div>
    <script src="./js/dep.js"></script>
    <script src="./js/watcher.js"></script>
    <script src="./js/compiler.js"></script>
    <script src="./js/observer.js"></script>
    <script src="./js/myVue.js"></script>
<script>
    const vm = new MyVue({
        el: '#app',
        data: {
            name: '张三',
            age: 50,
            son: {
                name: '李四',
                age: 25
            }
        }
    })
</script>
</body>
</html>
```
#### 页面展现
![](https://img.cmvideo.cn/publish/noms/2021/08/18/1O32J6GSNVNNP.png)
### 2、myVue类实现
js/myVue.js
```javascript
class MyVue {
  constructor(options) {
      // 将构建vue对象传入的参数存入vue实例
      this.$options = options || {};
      this.$data = options.data || {};
      // el可以是dom对象也可以是字符串
      this.$el = typeof options.el === 'string' ? document.querySelector(options.el) : options.el;
      // 将data中属性替换为getter，setter, 注入到vue实例中
      this._proxyData(this.$data);
      // 将data的属性变为响应式
      new Observer(this.$data);
      // 解析模板和指令
      new Compiler(this);
  }
  _proxyData(data) {
      Object.keys(data).forEach(key => {
          Object.defineProperty(this, key, {
              enumerable: true,
              configurable: true,
              set(v) {
                  if (v === data[key]) return;
                  data[key] = v;
              },
              get() {
                  return data[key];
              }
          })
      })
  }
}
```
### 3、Observer类实现
js/Observer.js
```javascript
class Observer {
  constructor(data) {
      this.walk(data);
  }
  walk(data) {
      if (!data || typeof data !== 'object') return;
      Object.keys(data).forEach(key => {
          this.defineReactive(data, key, data[key]);
      })
  }
  // 传入参数val,而不直接使用data[key]，是为了避免死循环触发get,set，导致内存溢出
  defineReactive(data, key, val) {
      let self = this;
      // 如果属性的值也是对象，应该将此对象的属性也变为响应式
      this.walk(val);
      // 给data的每一个属性创建一个发布者dep对象
      let dep = new Dep();
      Object.defineProperty(data, key, {
          enumerable: true,
          configurable: true,
          set(v) {
              if (v === val) return;
              val = v;
              // 如果修改的值为对象，则也需将此对象的属性转换为响应式
              self.walk(v);
              // data数据发生改变，触发set方法，通知发布者，执行notify方法，通知订阅者更新视图
              dep.notify();
          },
          get() {
              // 订阅者watcher实例创建时，会该实例赋值给Dep的静态属性target，同时会触发data[key]的get方法
              // 将订阅者存入该属性创建的发布者对象中
              Dep.target && dep.subs.push(Dep.target);
              return val;
          }
      })
  }
}
```
### 4、Compiler类实现
js/compiler.js
```javascript
class Compiler {
    constructor(vm) {
        this.vm = vm;
        this.el = vm.$el;
        this.compile(this.el);
    }
    compile(el) {
        let childNodes = el.childNodes;
        Array.from(childNodes).forEach(node => {
            if (this.isTextNode(node)) {
                this.compileText(node);
            } else if (this.isElementNode(node))  {
                this.compileElement(node);
            }
            // 如果子节点还有子节点，需循环调用
            if (node.childNodes && node.childNodes.length) {
                this.compile(node);
            }
        })
    }
    // 处理文本节点差值表达式
    compileText(node) {
        let reg = /\{\{(.+?)\}\}/; // 正则匹配差值表达是{{ xxx }}
        let value = node.textContent;
        if (reg.test(value)) {
            let key = RegExp.$1.trim();
            node.textContent = value.replace(reg, this.vm[key]);
            // 为每个依赖data属性的地方，创建订阅者，当数据改变时，更新视图
            new Watcher(this.vm, key, newValue => {
                node.textContent = newValue;
            });
        }
    }
    // 处理元素节点
    compileElement(node) {
        let attrs = node.attributes;
        Array.from(attrs).forEach(attr => {
            let attrName = attr.name;
            if (this.isDirective(attrName)) {
                attrName = attrName.substr(2);
                let key = attr.value;
                this.update(node, key, attrName);
            }
        })
    }
    // 这样写的好处是避免在方法里写过多的if判断，以后新增指令解析只需新增xxxUpdater方法即可
    update(node, key, attrName) {
        let updateFn = this[attrName + 'Updater'];
        updateFn && updateFn.call(this, node, this.vm[key], key);
    }
    // 处理v-text
    textUpdater(node, val, key) {
        node.textContent = val;
        // 为每个依赖data属性的地方，创建订阅者，当数据改变时，更新视图
        new Watcher(this.vm, key, newValue => {
            node.textContent = newValue;
        })
    }
    // 处理v-html
    htmlUpdater(node, val, key) {
        // 为每个依赖data属性的地方，创建订阅者，当数据改变时，更新视图
        node.innerHTML = val;
        new Watcher(this.vm, key, newValue => {
            node.innerHTML = newValue;
        })
    }
    // 处理v-model
    modelUpdater(node, val, key) {
        node.value = val;
        // 为每个依赖data属性的地方，创建订阅者，当数据改变时，更新视图
        new Watcher(this.vm, key, newValue => {
            node.value = newValue;
        })
        // 双向数据绑定
        node.addEventListener('input', () => {
            this.vm[key] = node.value;
        })
    }
    // 判断当前节点是否是文本节点
    isTextNode(node) {
        return node.nodeType === 3;
    }
    // 判断当前节点是否是元素节点
    isElementNode(node) {
        return node.nodeType === 1;
    }
    // 判断当前元素是否是vue指令
    isDirective(attrName) {
        return attrName.startsWith('v-');
    }
}
```
### 5、发布者Dep类实现
js/dep.js
```javascript
class Dep {
    constructor() {
        // 存储订阅者
        this.subs = [];
    }
    // 添加订阅者
    addSub(sub) {
        // 订阅者都有一个update方法
        if (sub && sub.update) {
            this.subs.push(sub);
        } 
    }
    // 数据发生变化时通知当前实例存入的所有订阅者，执行订阅者的update方法
    notify() {
        this.subs.forEach(sub => {
            sub.update();
        })
    }
}
```
### 6、订阅者Watcher类实现
js/watcher.js
```javascript
class Watcher {
    // 创建实例接收三个参数，cb是触发update时执行的回调
    constructor(vm, key, cb) {
        this.vm = vm;
        this.key = key;
        this.cb = cb;
        // 将当前订阅者对象赋值给发布者对象的静态属性target，在执行date[key]的get方法时，将订阅者对象存入发布者对象的subs属性里
        Dep.target = this;
        this.oldValue = vm[key];
        // 清空target属性，避免多次存入
        Dep.target = null;
    }
    update() {
        let newValue = this.vm[this.key];
        if (newValue === this.oldValue) return;
        this.cb(newValue);
    }
}
```
### 7、最终效果
![image.png](https://i.loli.net/2021/08/19/j8Ssxd2IDU6R1qc.png)

![image.png](https://i.loli.net/2021/08/19/6Ljn8AhCFVysqHk.png)

![image.png](https://i.loli.net/2021/08/19/BztLhRKxUVEMX5c.png)

## 结语
本次通过代码模拟了vue的响应式编程原理，其中涉及到数据劫持、双向绑定、观察者模式等技术