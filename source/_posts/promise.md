---
title: promise原理剖析及模拟实现
date: 2021-08-16 18:39:37
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
        executor(this.resolve, this.reject)
    }
    status = PENDING;
    value = undefined; // 执行成功后返回的值
    reason = undefined; // 执行失败互殴返回的原因
    resolve = (val) => {
        // 如果状态不是等待，表示状态已经改变，不可再进行修改
        if (this.status !== PENDING) return;
        // 将状态变为成功
        this.status = FULFILLED;
        // 接受成功后传递的值
        this.value = val;
    }
    reject = (reason) => {
        // 如果状态不是等待，表示状态已经改变，不可再进行修改
        if (this.status !== PENDING) return;
        // 将状态变为失败
        this.status = REJECTED;
        // 接受失败后传递的原因
        this.reason = reason;
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

