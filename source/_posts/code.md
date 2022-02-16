---
title: code
date: 2022-02-16 15:33:35
cover: https://s4.ax1x.com/2022/02/16/Hf746f.png
tags:
- js
---

## 手撕代码

### 防抖节流
防抖：n秒内多次触发，会重新计时，最终只执行一次
```javascript
function fd(fn, wait, immediate) {
    let timer = null;
    return function () {
        let args = Array.from(arguments);
        if (immediate && !timer) {
            fn.call(this, ...args)
        }
        if (timer) {
            clearTimeout(timer);
        }
        timer = setTimeout(() => {
            fn.call(this, ...args)
        }, wait)
    }
}

function handler() {
    console.log('我是防抖的，我1s内只执行一次')
}

let say = fd(handler, 1000, true);

window.addEventListener('mousemove', say)
```

节流：n秒内执行一次
```javascript
//不使用setTimeout
function jl_pre(fn, wait) {
    let pre = 0;
    return function () {
        let args = Array.from(arguments);
        let now = new Date().getTime();
        if (now - pre >= wait) {
            pre = now;
            fn.call(this, ...args);
        }
    }
}

// 使用setTimeout
function jl_timer(fn, wait) {
    let lock = false;
    return function () {
        if (lock) return;
        let args = Array.from(arguments);
        lock = true;
        setTimeout(() => {
            lock = false;
            fn.call(this, ...args);
        }, wait)
    }
}

function handler() {
    console.log('我是节流的，持续触发，我每1s内只执行一次')
}

let say = jl_pre(handler, 1000);

let sayTime = jl_timer(handler, 1000)

window.addEventListener('mousemove', sayTime)

```

### 深拷贝
```javascript
function deepClone(obj) {
    let deepObj = Array.isArray(obj) ? [] : {};
    if (typeof obj === 'object') {
        for (let key in obj) {
            deepObj[key] = typeof obj[key] === 'object' ? deepClone(obj[key]) : obj[key];
        }
    } else {
        deepObj = obj;
    }
    return deepObj;
}
let a = {
    name: 'zs',
    age: 30,
    son: {
        name: 'zw',
        age: 1
    }
}
let b = deepClone(a);
```

### 数组去重
```javascript
let arr = [1,2,3,3,5,8,2,1,8,2,6];
// 方法1 使用set特性
let arr1 = [...new set(arr)];

// 方法2 使用reduce方法
let arr2 = arr.reduce((prev, cur) => {
    if (prev.includes(cur)) {
        return prev
    } else {
        return prev.concat(cur);
    }
}, [])

// 方法3 使用循环
let arr3 = [];
for (let key in arr) {
    if (!arr3.includes(arr[key])) {
        arr3.push(arr[key]);
    }
}
```

### 计算数组每个元素出现的次数
```javascript
let arr = ['tom', 'mick', 'josn', 'tom', 'a', 1, 1, 'a', 'tom']

let obj = arr.reduce((prev, cur) => {
    if (prev[cur]) {
        prev[cur]++;
    } else {
        prev[cur] = 1
    }
    return prev;
}, {})

console.log(obj)
```

### 将二维数组拍平成一维数组
```javascript
let arr = [[1,2],[3,4],[5,6]]

let arr1 = arr.reduce((prev, cur) => {
    return prev.concat(cur);
}, [])

console.log(arr1)
```

### 将多维数组拍平成一维数组
```javascript
let arr = [1,[1,2],[1,[1,2,3,[4,5]]]]

let newArr = function (arr) {
    return arr.reduce((prev, cur) => {
        return prev.concat(Array.isArray(cur) ? newArr(cur) : cur);
    }, [])
}

console.log(newArr(arr))
```