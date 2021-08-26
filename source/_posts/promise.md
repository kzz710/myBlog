---
title: promise原理剖析及模拟实现
date: 2021-08-10 18:39:37
tags: 
    - promise 
cover: http://ww3.sinaimg.cn/large/d2e27164gw1fbmwbgf0mij21hc0u0487.jpg
---

## promise原理解析及模拟实现
### promise原理
1、Promise就是一个类，在构建Promise实例时需要传递一个立即执行的执行器方法  
2、执行器方法接收两个参数，分别是resolve方法和reject方法  
3、Promise执行过程中，存在三种状态，分别为：成功 fulfilled，失败 rejected，等待 pending  
4、Promise的resolve、reject方法是用来改变状态的 resolve: pending => fulfilled, reject: pending => rejected，一旦状态确定就不可修改
5、Promise的then方法是用来判断当前状态，成功则执行成功回调，失败则执行失败回调  
6、then方法分别接受成功回调和失败回调两个参数，成功回调函数有一个参数，表示成功之后的值，失败回调函数有一个参数，表示失败的原因

### 代码实现
#### 1、初步实现
```javascript
// --------------Promise代码--------------
const PENDING = 'pending';
const FULFILLED = 'fulfilled';
const REJECTED = 'rejected';
class MyPromise {
    constructor(executor) {
        this.status = PENDING;
        this.value = undefined; // 执行成功后返回的值
        this.reason = undefined; // 执行失败互殴返回的原因
        this.resolve = (val) => {
            // 如果状态不是等待，表示状态已经改变，不可再进行修改
            if (this.status !== PENDING) return;
            // 将状态变为成功
            this.status = FULFILLED;
            // 接受成功后传递的值
            this.value = val;
        }
        this.reject = (reason) => {
            // 如果状态不是等待，表示状态已经改变，不可再进行修改
            if (this.status !== PENDING) return;
            // 将状态变为失败
            this.status = REJECTED;
            // 接受失败后传递的原因
            this.reason = reason;
        }
        executor(this.resolve, this.reject);
    }
    // then方法接受成功回调和失败回调两个参数，如果调用时状态为成功则执行成功的回调，状态为失败则执行失败的回调
    then(successCallback, failCallback) {
        if (this.status === FULFILLED) {
            successCallback(this.value);
        } else if (this.status === REJECTED) {
            failCallback(this.reason);
        }
    }
}

// --------------调用--------------
let promise = new MyPromise((resolve, reject) => {
    let i = 2;
    if (i % 2 === 0) {
        resolve(i);
    } else {
        reject('不是偶数')
    }
})
promise.then((val) => {
    console.log(val);
}, (reason) => {
    console.log(reason);
})
```
思考：  
1、实际应用promise大都用于请求接口，获取返回值，再resolve或reject结果，这种异步流程  
2、如果resolve和reject是异步调用，那多次执行then方法  
上面代码是否满足这两种情况？

#### 2、二次实现
```javascript
// --------------Promise代码--------------
/**
 * 注释原因
 * 1、无法满足异步情况下，多次调用then方法的情况，最后一次的回调会将前面的回调覆盖
 */

const PENDING = 'pending';
const FULFILLED = 'fulfilled';
const REJECTED = 'rejected';
class MyPromise {
    constructor(executor) {
        this.status = PENDING;
        this.value = undefined; // 执行成功后返回的值
        this.reason = undefined; // 执行失败互殴返回的原因

        // 注释原因(1)
        // this.successCallback = undefined; // 成功的回调
        // this.failedCallback = undefined; // 失败的回调

        // 兼用异步情况下，多次调用then方法
        this.successCallback = []; // 存入成功的回调
        this.failedCallback = []; // 存入失败的回调
        this.resolve = (val) => {
            // 如果状态不是等待，表示状态已经改变，不可再进行修改
            if (this.status !== PENDING) return;
            // 将状态变为成功
            this.status = FULFILLED;
            // 接受成功后传递的值
            this.value = val;
            // 只用在异步调用resolve时，this.successCallback才不为空，才调用
            // 注释原因(1)
            // this.successCallback && this.successCallback(this.value);

            while (this.successCallback.length) this.successCallback.shift()(this.value);
        }
        this.reject = (reason) => {
            // 如果状态不是等待，表示状态已经改变，不可再进行修改
            if (this.status !== PENDING) return;
            // 将状态变为失败
            this.status = REJECTED;
            // 接受失败后传递的原因
            this.reason = reason;
            // 只用在异步调用reject时，this.failCallback才不为空，才调用
            // 注释原因(1)
            // this.failCallback && this.failCallback(this.reason);

            while (this.failedCallback.length) this.failedCallback.shift()(this.reason);
        }
        executor(this.resolve, this.reject);
    }
    // then方法接受成功回调和失败回调两个参数，如果调用时状态为成功则执行成功的回调，状态为失败则执行失败的回调
    then(successCallback, failCallback) {
        if (this.status === FULFILLED) {
            successCallback(this.value);
        } else if (this.status === REJECTED) {
            failCallback(this.reason);
        } else { // 异步resolve或reject时，执行then方法时，当前状态为pending
            // 将当前回调函数先存起来，在resolve和reject执行时再调用

            // 注释原因(1)
            // this.successCallback = successCallback;
            // this.failCallback = failCallback;

            this.successCallback.push(successCallback);
            this.failedCallback.push(failCallback);
        }
    }
}

// --------------调用--------------
let promise = new MyPromise((resolve, reject) => {
    let i = 2;
    if (i % 2 === 0) {
        setTimeout(() => {
            resolve(i);
        }, 2000)
    } else {
        setTimeout(() => {
            reject('不是偶数');
        }, 2000)
    }
})
promise.then((val) => {
    console.log('第一次',val);
}, (reason) => {
    console.log(reason);
})
promise.then((val) => {
    console.log('第二次',val);
}, (reason) => {
    console.log('第二次', reason);
})
```
思考：  
1、then方法链式调用  
2、then方法链式调用可以不传回调  
上面代码是否满足？ 

```javascript
// then方法链式调用，后面then执行回调都是前面then方法的返回值，看返回值是普通值还是promise对象  
// 如果是promise需要判断promise执行resolve还是reject，来执行相应的回调
```




