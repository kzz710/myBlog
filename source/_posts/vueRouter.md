---
title: vueRouter原理剖析
date: 2021-08-13 18:39:37
tags: 
    - vue 
    - 单页面 
    - 路由
cover: http://gss0.baidu.com/9fo3dSag_xI4khGko9WTAnF6hhy/zhidao/pic/item/32fa828ba61ea8d3ef5ed4599c0a304e251f586a.jpg
---
## 原理分析
1、VueRouter是一个插件类，能够被Vue.use()调用，即类的内部有一个静态方法install  
2、VueRouter类内部有三个成员，即options负责接收构建实例时传入的参数，routeMap负责将options中将路由规则routes以键值对的方法存起来，data中有一个current响应式属性用来改变路由切换  
3、VueRouter类内部存在三个方法，initComponents用来解析路由指令router-link和router-view，createRouteMap负责将路由规则存储，initEvent监听浏览器的回退实现路由跳转  

## 代码实现
```javascript
let _Vue = null;
export default class VueRouter {
    // 供Vue.use调用
    static install(Vue) {
        // 判断当前插件是否安装，即install方法是否被调用，若已调用则不再执行安装
        if (VueRouter.install.installed) {
            return;
        } 
        VueRouter.install.installed = true;
        // 将传入Vue存入到全局，方便后面方法调用
        _Vue = Vue; 
        // 把创建Vue实例时传入的router对象注入到Vue实例上
        // 混入
        Vue.mixin({
            beforeCreate() {
                // 此处的this指向Vue实例
                if (this.$options.router) { // 只在Vue构建实例时执行，在组件内部不执行
                    Vue.prototype.$router = this.$options.router;
                    this.$options.router.init();
                } 
            }
        })
     }
     constructor(options) {
        this.options = options;
        this.routeMap = {};
        this.data = _Vue.observable({ // 将data中属性变为响应式数据
            current: '/'
        })
    }
    init() {
        this.createRouteMap();
        this.initComponents();
        this.initEvent();
    }
    createRouteMap() {
        // 将传入的路由规则path和组件component按照键值对的形式存入routeMap中
        this.options.routes.forEach(route => {
            this.routeMap[route.path] = route.component;
        })
    }
    initComponents() {
        let self = this;
        // 定义router-link组件
        _Vue.component('router-link', {
            props: { // 接受的参数
                to: String
            },
            render(h) { // 渲染模板，渲染成a标签
                return h('a', {
                    attrs: {
                        href: this.to
                    },
                    on: {
                        click: this.clickHandle
                    }
                }, [this.$slots.default]) // router-link中的内容
            },
            methods: {
                 clickHandle(e) {
                     // 将当前路径存入浏览器历史中
                     window.history.pushState({}, '', this.to);
                     // 改变当前路径
                     this.$router.data.current = this.to;
                     // 阻止a标签的默认行为
                     e.preventDefault();
                 }
            }
        });
        // 定义router-view
        _Vue.component('router-view', {
            render(h) { // 根据当前路径获取到当前路径对应的组件component，渲染到页面中
                let temp = this.$router.routeMap[self.data.current];
                return h(temp);
            }
        })
    }
    initEvent() {
        // 监听浏览器回退、前进按钮，获取当前路径，由于current为响应式数据，数据改变，当前组件也会改变
        window.addEventListener('popstate', () => {
            this.data.current = window.location.pathname;
        })
    }
}
```
## 代码验证
1、使用Vue脚手架工具，构建一个带有vueRouter的项目
```bash
npm install -g @vue/cli

vue create my-router-view

Vue CLI v4.5.13
? Please pick a preset:
  Default ([Vue 2] babel, eslint)
  Default (Vue 3) ([Vue 3] babel, eslint)
> Manually select features

Vue CLI v4.5.13
? Please pick a preset: Manually select features
? Check the features needed for your project:
 (*) Choose Vue version
 (*) Babel
 ( ) TypeScript
 ( ) Progressive Web App (PWA) Support
>(*) Router
 ( ) Vuex
 ( ) CSS Pre-processors
 (*) Linter / Formatter
 ( ) Unit Testing
 ( ) E2E Testing
 
 Vue CLI v4.5.13
 ? Please pick a preset: Manually select features
 ? Check the features needed for your project: Choose Vue version, Babel, Router, Linter
 ? Choose a version of Vue.js that you want to start the project with
 > 2.x
   3.x

```
2、将实现代码写入myViewRouter.js中
3、将router/index.js中引入VueRouter替换为自己实现的myViewRouter
```javascript
import VueRouter from '../myViewRouter/myViewRouter' // 引入自己的路径
```
4、开始运行模拟实现

