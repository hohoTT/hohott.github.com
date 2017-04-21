---
layout:     post
title:      "Vue 前端 MVVM 框架使用教程（二）"
subtitle:   " \"Vue + Webpack\""
date:       2017-04-20 11:00:00
author:     "hohoTT"
header-img: "img/writing_img/2017-4-20/post-bg.jpg"
catalog: true
tags:
    - Java
    - Web
    - Vue
    - Js
---


## Vue 的组件使用

### 创建一个组件

在工程目录`/src`下创建 `components` 文件夹，并在 `components` 文件夹下创建一个 `firstComponent.vue` 并写仿照 `App.vue` 的格式和前面学到的知识写一个组件。

以下为 `firstComponent.vue` 文件

```html
<template>
<div id="firstcomponent">
    <h1>This is a firstComponent's title.</h1>
    <a> written by {{ author }} </a>
</div>
</template>

<script type="text/javascript">
    export default {
        data () {
            return {
                author: "hohoTT"
            }
        }
    }
</script>

<style>
</style>
```



### 使用组件


在 `App.vue` 文件中使用组件 ( 因为在 `index.html` 里面定义了 `<div id="app"></div>` 所以就以这个组件作为主入口，方便 )

- 第一步：引入
在 `<script></script>` 标签内的第一行填写以下代码

```html
import firstComponent from './components/firstComponent.vue'
```

- 第二步：注册
在 `<script></script>` 标签内的 `data` 代码块后面加上 `components: { firstComponent }`。

```html
export default {
    data () {
        return {
            author: "hohoTT"
        }
    }
}
```

- 第三步：使用
在 `<template></template>` 内加上 `<firstComponent></firstComponent>`

```html
<div id="firstcomponent">
    <h1>This is a firstComponent's title.</h1>
    <a> written by {{ author }} </a>
</div>
```

完成后 `app.vue` 代码示例

```html
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <h1>hohoTT</h1>
    <p>{{msg}}</p>
    <firstcomponent></firstcomponent>
    <ul>
      <li><router-link to="/first">点我跳转到第一页</router-link></li>
      <li><router-link to="/second">点我跳转到第二页</router-link></li>
    </ul>
    <router-view class="view"></router-view>
  </div>
</template>

<script>

  // 引入组件
  import firstcomponent from './components/firstComponent.vue'

  export default {
    name: 'app',
    data () {
      return {
        msg: 'Welcome to Your Vue.js App'
      }
    },
    components: { firstcomponent }
  }
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
```


之后页面呈现为以下效果

<div align="center"><img src="http://www.hohott.wang/img/writing_img/2017-4-20/s2.png"/></div>


## 使用路由搭建单页应用前期准备

### 路由安装

路由的 **安装命令** 介绍
安装 **路由依赖**

```java
npm install vue-resource 安装路由的最基本命令
npm install vue-resource -D 这个命令是在 webpack.config.js 中加入别名
```

<div align="center"><img src="http://www.hohott.wang/img/writing_img/2017-4-20/s1.png"/></div>

### 问题解决

出现的问题可以有以下几点 **解决办法**：
尝试使用 **国内镜像** 进行下载安装

```java
cnpm install vue-resource 
cnpm install vue-resource -D 
```

一般是一开始就执行 **npm install vue-resource -D** 命令，但是有些时候可能出现错误，所以可以先执行 **npm install vue-resource**，之后再执行 **npm install vue-resource -D** 命令,这样问题一般可以得到解决
最后主要实现 `package.json` 中对 **vue-resource** 的注册使用
<div align="center"><img src="http://www.hohott.wang/img/writing_img/2017-4-20/s3.png"/></div>

## 路由搭建使用以及界面呈现结果

### 新建组件
按照之前介绍的新建 **第二个组件** `secondComponent.vue`

```html
<template>
    <div id="secondcomponent">
        <h1>This is the second page</h1>
        <a>second page written by {{ author }} </a>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                author: "hohoTT",
                articles: [],
            }
        }
    }
</script>

<style>
</style>
```

### 修改 main.js 文件
修改 `main.js`，引入并注册 `vue-router`

```html
import Vue from 'vue'
import App from './App.vue'
import VueRouter from "vue-router";
import VueResource from 'vue-resource'

//开启debug模式
Vue.config.debug = true;

Vue.use(VueRouter);
Vue.use(VueResource);

// 定义组件, 也可以像教程之前教的方法从别的文件引入
const First = {
  template: '<div><h1>This is the first page</h1><a>First page written by hohoTT </a></div>'
}

import secondComponent from './components/secondComponent.vue'

// 创建一个路由器实例
// 并且配置路由规则
const router = new VueRouter({
    mode: 'history',
    base: __dirname,
    routes: [
        {
            path: '/first',
            component: First
        },
        {
            path: '/second',
            component: secondComponent
        }
    ]
})

// 现在我们可以启动应用了！
// 路由器会创建一个 App 实例，并且挂载到选择符 #app 匹配的元素上。
const app = new Vue({
    router: router,
    render: h => h(App)
}).$mount('#app')
```

## 界面效果图以及工程结构图

### 首页
<div align="center"><img src="http://www.hohott.wang/img/writing_img/2017-4-20/r1.png"/></div>

### first 组件
<div align="center"><img src="http://www.hohott.wang/img/writing_img/2017-4-20/r2.png"/></div>

### second 组件
<div align="center"><img src="http://www.hohott.wang/img/writing_img/2017-4-20/r3.png"/></div>

### 项目工程结构图
<div align="center"><img src="http://www.hohott.wang/img/writing_img/2017-4-20/r4.png"/></div>

## 著作权声明
本文首次发布于 [hohoTT's Blog](http://www.hohott.wang/)，转载请保留 [http://www.hohott.wang/2017/04/20/Vue-Summary-Second/](http://www.hohott.wang/2017/04/20/Vue-Summary-Second/) 以上链接并附上作者 **hohoTT**。如果您喜欢，还可以关注我的微信公众号 **hohoTT** , 或者扫一扫下方的微信公众号二维码即可 ^_^。
<div align="center"><img src="http://www.hohott.wang/img/WeiXinImg.jpg"/></div>



