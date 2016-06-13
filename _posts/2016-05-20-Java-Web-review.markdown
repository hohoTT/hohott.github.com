---
layout:     post
title:      "Java Web 有关的知识点回顾复习（上）"
subtitle:   " \"Java Web\""
date:       2016-05-20 10:00:00
author:     "hohoTT"
header-img: "img/writing_img/2016-5-20/post-bg.jpg"
catalog: true
tags:
    - Java
    - Web
    - Review
---

## web开发历程

#### CGI:

**CGI** (Common Gateway Interface)是 **WWW技术** 中最重要的技术之一，有着不可替代的重要地位。**CGI** 是 `外部应用程序` （**CGI程序**）与 **Web服务器** 之间的 `接口标准`，是在 **CGI程序** 和 **Web服务器** 之间传递信息的规程。**CGI** 规范允许 **Web服务器** 执行外部程序，并将它们的输出发送给 **Web浏览器**，**CGI** 将 **Web** 的一组简单的 `静态超媒体文档` 变成一个完整的新的 `交互式媒体`。

#### ODBC:

`开放数据库互连`（Open Database Connectivity，**ODBC**）是微软公司 `开放服务结构`（WOSA，Windows Open Services Architecture）中有关数据库的一个组成部分，它建立了一组规范，并提供了一组对 `数据库访问` 的标准 **API**（`应用程序编程接口`）。

1997年, `Sun公司` 退出了 **Servlet规范** ,java有了web开发的利器.1998年 **JSP技术** 产生, **Servlet** , **JSP** 加上 **javaBean** 让 **JavaWeb开发** 同时拥有了 **CGI** 的 `集中处理能力` 和 **PHP** 的嵌入 **html** 的功能.1998年 `Sun公司` 发布了 **EJB1.0** 的规范,99年 `Sun` 发布 **J2EE** 的第一个版本.2001年微软提出了 **ASP.NET技术**。

## JAVA Web 应用的结构

<div align="center"><img src="http://www.hohott.wang/img/writing_img/2016-5-20/img1.png"/></div>

## Servlet

**Servlet容器** 为 **JavaWeb应用** 提供运行时环境,它负责管理 **servlet** 和 **JSP** 的 `声明周期` ,以及管理他们的 `共享数据` .也称 **JavaWeb** 的 `应用容器`。

目前最流行的 **servlet容器** 软件有: `Tomcat(apache)` , `Resin` , `J2EE服务器` (Weblogic,websphere,jboss)也内置了 **Servlet容器**, **EJB**.

**Servlet** 在 **Web应用** 中担任重要角色。**Servlet** 运行于 **Servlet容器** 中，可以被 **Servlet容器** 动态加载，来 `扩展服务器` 的功能，并提供特定的服务。**Servlet** 按照 `请求/响应` 的方式工作。在 **Struts** 和 **Tapestry** 框架中，`控制器组件`就是由 **Servlet** 来构成的。

当用户 `请求访问`某个 **Servlet** 时，**Servlet容器** 将创建一个 **ServletRequest对象** 和 **ServletResponse对象** 。在 **ServletRequest对象** 中封装了 `用户请求信息` ，然后 **Servlet容器** 把 **ServletRequest对象** 和 **ServletResponse对象** 传给用户所请求的 **Servlet**。**Servlet** 把响应结果写入**ServletResponse** 中，然后由 **Servlet容器** 把响应结果传给用户。

#### Serlet容器
**Serlet容器** 响应用户请求的过程: 
 
<div align="center"><img src="http://www.hohott.wang/img/writing_img/2016-5-20/img2.png"/></div>

#### Java Servlet API
在 **Java Servlet API** 中有以下几个比较重要的类，它们决定了 **Web应用** 的 `请求/响应` 方式及各种共享数据的存放地点：
 
 - HttpServletRequest: **Servlet容器** 把 **HTTP请求信息** 包含在 **HttpServletRequest对象** 中，**Servlet组件** 从 **request对象** 中读取用户的 `请求数据`。此外，**HttpServletRequest** 可以存放 **request** 范围内的 `共享数据`。 
 - HttpServletResponse：用户生成 **HTTP响应** 结果。
 - HttpSession ： **Servlet容器** 为每个 **HTTP会话** 创建一个 **HttpSession实例** ，**HttpSession** 可以存放 **session范围** 的共享数据。
 - ServletContext：
**Servlet容器** 为每个 **Web应用** 创建一个 **ServletCntext实例**，**ServletCntext** 可以存放 **application范围** 的共享数据

## JSP

在传统的 **HTML文件** 中加入 **Java程序片段** 和 **JSP标签** ，就构成了 **JSP网页**。

当 **JSP容器** 接收到 **Web用户** 的一个 **JSP文件请求** 时，它对 **JSP文件** 进行语法分析并生成 **Java Servlet源文件**，然后 `对其编译`。一般情况下。**Servlet源文件**的 `生成` 和 `编译` 仅在 `初次` 调用 **JSP** 时发生。如果原始的 **JSP文件** 被更新，**JSP容器** 将检测所做的更新，在执行它之前重新生成 **Servlet** 并进行 `编译`。

**JSP容器** 初次执行 **JSP** 的过程：

<div align="center"><img src="http://www.hohott.wang/img/writing_img/2016-5-20/img3.png"/></div>

## 著作权声明
本文首次发布于 [hohoTT's Blog](http://www.hohott.wang/)，转载请保留 [http://www.hohott.wang/2016/05/20/Java-Web-review/](http://www.hohott.wang/2016/05/20/Java-Web-review/) 以上链接并附上作者 **hohoTT**。如果您喜欢，还可以关注我的微信公众号 **hohoTT** , 或者扫一扫下方的微信公众号二维码即可 ^_^。
<div align="center"><img src="http://www.hohott.wang/img/WeiXinImg.jpg"/></div>
