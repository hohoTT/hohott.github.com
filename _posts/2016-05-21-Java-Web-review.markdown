---
layout:     post
title:      "Java Web 有关的知识点回顾复习（下）"
subtitle:   " \"Java Web\""
date:       2016-05-21 11:00:00
author:     "hohoTT"
header-img: "img/writing_img/2016-5-21/post-bg.jpg"
catalog: true
tags:
    - Java
    - Web
    - Review
---


## 共享数据在Web应用中的范围

`共享数据` 有 **4种存在范围**。

#### Page

**Page**：`共享数据` 的 `有效范围` 是 `用户请求访问` 的当前页面。

#### Request

**Request**：`共享数据` 的 `有效范围` 为 `用户请求访问` 的当前 **Web组件**，以及和 **当前Web组件** `共享` **同一个用户请求** 的其他 **Web组件**。如果 `用户请求访问` 的是 **JSP网页**，那么该 **JSP网页** 的 `<%@include>指令` 以及 `<forward>标记` 所包含的 **其他JSP文件** 也能访问 `共享数据` 。**Request范围** 内的 `共享数据` 实际上存放在 **HttpServletRequest对象** 中。

#### Session

`共享数据` 存在于 **整个HTTP会话** 的 `生存周期` 内， **同一个HTTP会话** 中的 **Web组件** 共享它。**Session范围** 内的 `共享数据` 实际上是存放在 **HttpSession对象** 中。

#### Application

`共享数据` 存在于整个 **Web应用** 的 `生命周期` 内， **Web应用** 中的 **所有Web组件** 都能 `共享` 它， `共享数据` 实际上存放在 **ServletCntext对象** 中。

`共享数据` 在 **Web应用** 中的范围:

<div align="center"><img src="http://www.hohott.wang/img/writing_img/2016-5-20/img4.png"/></div>

当客户第一次访问 **Web应用** 中支持会话的某个网页时们就会开始一个 **新的HTTP会话**， **Servlet容器** 为这个 `会话` 创建一个 **HttpSession对象**。接下来，当客户浏览这个 **Web应用** 的 **不同网页** 时，始终处于同一个会话中。会话拥有特定的 `生命周期`。

在以下情况中，会话将结束 `生命周期` ，**Servlet容器** 会将 `HTTP会话` `所占有` 的 `资源释放掉`：

 - `客户端` 关闭 `浏览器`

 - **会话过期**

 - `服务器端` 调用了 **HttpSession** 的 **invalidate()方法**

## JavaBean组件

**JavaBean** 是一种符合 `特定规范` 的 **JAVA 对象**。在 **JavaBean** 中定义了一系列的属性，并提供了 `访问` 和 `设置` 这些属性的 **公用方法**。**JavaBean** 可以作为 `共享数据` ，存放在 `page`，`request`，`session`，`application`范围内。在 **JSP文件** 中，可以通过 **专门的标签** 来 `定义` 和 `初始化` **JavaBean**。如

```html
<jsp:useBean id="calendar" scope="page/request/session/application" class="employee.Calendar" />

<h2>Calendar of <jsp:getProperty name="calendar" property="username" /></h2> 
```

## EJB组件

**EJB组件** 是基于标准 `分布式对象` 技术， **CORBA** 和 **RMI** 的 `服务端` **JAVA组件**，**EJB组件** 和 **JavaBean组件** 一样，都用于实现 `企业应用` 的 **业务逻辑**，它们的根本区别在于，**EJB组件** 总是 `分布式` 的，**EJB组件** 运行于 **EJB服务器** 中，而 **JavaBean组件** 可以和 **Servlet** 或 **JSP** 运行于 **同一JAVA虚拟机** 中。

## Web组件的三种关联关系

 - 请求转发

 - URL重定向

 - 包含

#### 请求转发

`请求转发` 允许把请求转发给 **同一应用** 中的 **其他Web组件**。这种技术通常用于 **Web应用控制层** 的 **Servlet流程控制器** ，它检查 **HTTP请求数据** ，并将 `请求` 转发到合适的 **目标组件**，**目标组件** 执行具体的 `请求处理操作` ，并生成 `响应结果`。

一个 **Servlet** 把 `请求` 转发给 `另一个` **JSP组件** 的过程:

<div align="center"><img src="http://www.hohott.wang/img/writing_img/2016-5-20/img5.png"/></div>

如果在 **Servlet组件** 转发请求给一个 **JSP组件**，可以在 **Servlet** 的 **service()方法** 中执行以下代码：

```java
RequestDispatcher rd = request.getRequestDispatcher(“hello.jsp”);

Rd.forward(request.response); 
```

如果在 **JSP页面** 中，可以使用<jsp:forward>标签来转发请求,例如：

```html
<jsp:forward page="hello.jsp"/>
```

对于 `请求转发`，转发的 `源组件` 和 `目标组件` 共享 **request范围** 内的数据。

#### 请求重定向

`请求重定向` 类似 `请求转发` ，但是有一些重要区别：

**Web组件** 可以将 **请求重定向** 到 `任一URL`，而 `不仅仅` 是 **同一应用中的URL**。

重定向的 **元组件** 和 **目标组件** 之间不共用同一个 **HttpServletRequest对象**，因此不能共享 **request范围** 内的 `共享数据`。

显示一个 **Servlet** 把 `请求重定向` 给另一个 **JSP组件** 的过程:

<div align="center"><img src="http://www.hohott.wang/img/writing_img/2016-5-20/img6.png"/></div>

如果当前应用的 **Servlet组件** 要把请求转发给 **URL**。可以在 **Servlet** 的 **service方法** 中执行：
```java
response.sendRedirect(URL);
```

#### 包含

`包含关系` 允许一个 **Web组件** 聚集来自 `同一个应用` 中的 **其他Web组件** 的输出数据，并使用被聚集的数据来创建 `响应结果`。这种技术通常用于 `模板处理器`，它可以 `控制网页的布局`。模板中每个页面区域的内容都来自 `不同的URL`，从而组成单个页面。

显示了一个 **Servlet** 包含另一个 **JSP组件** 的过程: 

<div align="center"><img src="http://www.hohott.wang/img/writing_img/2016-5-20/img7.png"/></div>
 
**Servlet类** 使用 **javax.servlet.RequestDispatcher.include()方法** 包含其他的 **Web组件**。

例如，如果当前 **Servlet组件** 包含了三个 **JSP文件**：`header.jsp`, `main.jsp` 和 `footer.jsp`,则在 **Servlet** 的 **service()方法**中执行如下代码：

```java
RequestDispatcher rd;

Rd=req.getRequestDispatcher(“/header.jsp”));

Rd.include(req,res);

Rd=req.getRequestDispatcher(“/main.jsp”));

Rd.include(req,res);

Rd=req.getRequestDispatcher(“/footer.jsp”));

Rd.include(req,res);
```

在 **JSP文件** 中，可以通过<include>指令来包含其他的Web资源，例如：

```html
<%@ include file=”header.jsp”%>

<%@ include file=”main.jsp”%>

<%@ include file=”footer.jsp”%>
```


## 著作权声明
本文首次发布于 [hohoTT's Blog](http://www.hohott.wang/)，转载请保留 [http://www.hohott.wang/2016/05/21/Java-Web-review/](http://www.hohott.wang/2016/05/21/Java-Web-review/) 以上链接并附上作者 **hohoTT**。如果您喜欢，还可以关注我的微信公众号 **hohoTT** , 或者扫一扫下方的微信公众号二维码即可 ^_^。
<div align="center"><img src="http://www.hohott.wang/img/WeiXinImg.jpg"/></div>

