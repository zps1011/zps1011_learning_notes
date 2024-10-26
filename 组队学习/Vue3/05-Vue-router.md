<div style="border-bottom: 4px solid black; width: 100%; box-sizing: border-box; text-align: center; padding-top: 0.1rem;" align="center">
    <h1>2024 年 10 月 Vue3 组队学习<br/><span>05. Vue-Router</span></h1>
</div>
<div style="text-align: center;" align="center">
    记录人：zps1011&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;记录时间：2024年10月26日
</div>



## Vue 路由基本概念

Vue路由是Vue.js官方提供的一种前端路由管理方式。它主要实现单页应用（SPA）的页面跳转和组件切换，允许以优雅的方式管理应用的URL。Vue Router 客户端路由的作用是在单页应用中，将浏览器的URL和用户看到的内容绑定起来。使用Vue Router，开发者可以配置静态或动态路由，拦截导航，并通过基于组件的配置方法进行详细导航控制。



## Vue-Router 路由模式

Vue-Router默认提供了两种路由模式，`history` 和 `hash` 模式。

#### hash 模式

Vue-Router 默认 hash 模式 —— 使用 URL 的 hash 来模拟一个完整的 URL，当 URL 改变时，页面不会重新加载，这样的好处是无需我们在服务端做任何配置就可以无缝接入我们的后端。`# `是 URL 的一个锚点，记载了网页中的位置，在实际的请求中，hash 并不会被带到后端。所以说 hash 模式通过锚点值的改变，根据不同的值，渲染指定DOM位置的不同数据。

```html
import {createWebHashHistory} from 'vue-router';
const router = createRouter({
history:createWebHashHistory(process.env.BASH_URL), //hash模式
route, //'routes:routes'的缩写
})
```


#### history 模式

由于 hash 模式本身会在我们的 URL 中带一串`hash`，而且还有 `#` 符号。如果需要较优雅的模式，那么可以使用history 模式。

```html
import {createRouter, createWebHistory} from 'vue-router';
const router = createRouter({
//history模式的实现
history:createWebHistory(process.env.BASE_URL),
route, //'routes:routes'的缩写
})
```


## Vue-Router 安装

和 vue 一样，vue-router 本身也提供了两种安装方式供我们选择。

#### CDN 方式

我们可以直接引入vue-router.js这个文件使用vue-router

```
<script src="/path/to/vue.js"></script>
<script src="/path/to/vue-router.js"></script>
```

#### NPM 方式

第二种方式则是在我们 Vue 项目中集成

```
npm install vue-router
```

之后在 main.js 中引入我们的 vue-router

```
import Vue from 'vue'
import App from './App.vue'
import router from './router'

new Vue({
  router,
  render: h => h(App)
}).$mount('#app')
```



## 编程式导航

在网页中，有以下两种界面跳转方式：

（1）使用 a 标签的形式，叫做标签跳转。

（2）使用 window.location.href 或者this.$router.push({})的形式，叫做编程式导航。

```html
parm的方式

// 不带参数
this.$router.push({name:'xxx'})

// 带参数
this.$router.push({name:'xxx',params:{key:value}})

query 的方式
this.$router.push({name:'xxx',query:{key:value}})

其它好玩的操作
this.$router.go(-1)//跳转到上一次浏览的页面
this.$router.replace('/menu')//指定跳转的地址
this.$router.replace({name:'menuLink'})//指定跳转路由的名字下
```



## 路由传参

路由传参通常有 query 和 params 两种方式。不管是哪一种方式，传参都是通过修改 URL 地址来实现的，路由对 URL 参数进行解析即可获取相应的参数。

（1）获取普通参数

对于/blogs?id=3 中的参数，我们可以这样获取：

```html
this.$route.query.id  //返回结果为3
```

（2）获取路由中定义好的参数

对于/blogs/3 这样的参数，可以对应的路由应该是：

```html
routes:[
 {
  path:'/blogs/:id',
  ...
 },
]
```

这个 named path 就可以通过下面的代码来获取 id。

```html
this.$route.params.id  //返回结果为3
```

#### params 和 query 的区别

- 使用 params 传参只能用 name 来引入路由，即push里面只能是name:'xxx',不能是path:'/xxx',因为params只能用name来引入路由，如果这里写成了path，接收参数页面会是undefined。query name 和 path 都支持。
- query 更类似于 get 请求，url 会加上参数信息，而params类似于post请求，url 并不会携带参数信息。



## 总结

本次主要介绍了Vue-router的一些简单使用，中间穿插了一些基础知识点，总体来说比较容易上手。使用localhost复现了部分功能，后续还需要更深入的精读与推敲。

