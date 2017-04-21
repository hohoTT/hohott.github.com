---
layout:     post
title:      "Vue 前端 MVVM 框架使用教程（一）"
subtitle:   " \"Vue + Webpack\""
date:       2017-04-20 11:00:00
author:     "hohoTT"
header-img: "img/writing_img/2017-4-21/post-bg.jpg"
catalog: true
tags:
    - Java
    - Web
    - Vue
    - Js
---


## 技术介绍

### 什么是Vue

<div align="center"><img src="http://www.hohott.wang/img/writing_img/2017-4-21/s1.png"/></div>

**Vue.js**（读音 `/vjuː/`，类似于 **view**） 是一套构建用户界面的**渐进式框架**。与其他重量级框架不同的是，`Vue` 采用自底向上增量开发的设计。`Vue` 的核心库只关注**视图层**，它不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与单文件组件和 `Vue` 生态系统支持的库结合使用时，`Vue` 也完全能够为复杂的单页应用程序提供驱动。
如果你是有经验的前端开发者，想知道 `Vue.js` 与其它库/框架的区别，查看对比其它框架。
其他具体的内容可查看**Vue官方文档** [https://cn.vuejs.org/](https://cn.vuejs.org/)

### Vue 特点
- 数据绑定
<div align="center"><img src="http://www.hohott.wang/img/writing_img/2017-4-21/s2.png"/></div>
- 组件化
<div align="center"><img src="http://www.hohott.wang/img/writing_img/2017-4-21/s3.png"/></div>

## Vue + Webpack 环境部署

### 安装环境

#### 安装 `node.js` 

前提你需要安装 `node.js`，请自行到 [https://nodejs.org/en/](https://nodejs.org/en/) 官方网站进行安装 `node.js`，之后你才可以进行下一步的工程部署

#### 安装淘宝镜像

```java
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

#### 安装 webpack

```java
cnpm install webpack -g
```

#### 安装 vue 脚手架

```java
npm install vue-cli -g
```

### 项目搭建

#### 进入项目工程目录开始创建项目

```java
vue init webpack-simple 工程名字<工程名字不能用中文>
```

之后按照提示，一步步直到创建项目工程完成
<div align="center"><img src="http://www.hohott.wang/img/writing_img/2017-4-21/vue_init.png"/></div>

### 项目运行

运行项目的初始**环境依赖**准备

#### 安装项目依赖

```java
npm install 
```

#### 安装 vue 路由模块vue-router和网络请求模块vue-resource

```java
cnpm install vue-router vue-resource --save
```

#### 启动项目

```java
npm run dev
```

- 注意：
  这里的启动可能会存在问题，一般是之前的依赖环境没有安装完全
- 解决方法：

```java
  尝试使用 cnpm install node-sass@latest 进行解决
```

如果一切正常，现在你已经可以访问你的初始界面了：）
<div align="center"><img src="http://www.hohott.wang/img/writing_img/2017-4-21/index.png"/></div>


### 常见问题

#### div 使用方面的错误

一个**组件**下只能有一个并列的 `div`，可以这么写，所以复制官网示例的时候只要复制 `div` 里面的内容就好。

 - 你可以这样写
 <div align="center"><img src="http://www.hohott.wang/img/writing_img/2017-4-21/k1.png"/></div>
 - 但是不能这样写
<div align="center"><img src="http://www.hohott.wang/img/writing_img/2017-4-21/k2.png"/></div>

#### Js 中的数据书写

- 数据要写在 **return** 里面
<div align="center"><img src="http://www.hohott.wang/img/writing_img/2017-4-21/k3.png"/></div>
- 以下为`错误`的写法
<div align="center"><img src="http://www.hohott.wang/img/writing_img/2017-4-21/k3.png"/></div>


## 著作权声明
本文首次发布于 [hohoTT's Blog](http://www.hohott.wang/)，转载请保留 [http://www.hohott.wang/2017/04/20/Vue-Summary-First/](http://www.hohott.wang/2017/04/20/Vue-Summary-First/) 以上链接并附上作者 **hohoTT**。如果您喜欢，还可以关注我的微信公众号 **hohoTT** , 或者扫一扫下方的微信公众号二维码即可 ^_^。
<div align="center"><img src="http://www.hohott.wang/img/WeiXinImg.jpg"/></div>


