---
layout:     post
title:      "Struts2 的 Action 中取得 Session 等对象的方法"
subtitle:   " \"Action 中访问 WEB 资源\""
date:       2016-04-27 20:00:00
author:     "hohoTT"
header-img: "img/writing_img/2016-4-27/post-bg.png"
catalog: true
tags:
    - Java
    - Struts2
    - SSH
---

## Action 中取得

 - 代码示例

```java
private Map<String,Object> session;  
private Map<String,Object> request;  
private Map<String,Object> application;  

session = ActionContext.getContext().getSession();  
request = (Map) ActionContext.getContext().get("request");  
application = ActionContext.getContext().getApplication();  

session.put("name", stu.getName());  
request.put("age", stu.getAge());  
application.put("name", stu.getName());  
```
 
## jsp 页面中取得

#### 一、 利用 Struts2 的标签

 - 代码示例

```html
<h2><s:property value="#session.name" /></h2>  
<h3><s:property value="#request.age" /></h3>  
<h4><s:property value="#application.name" /></h4>  
```

#### 二、 利用 Jsp 中默认对象

 - 代码示例

```html
<%=session.getAttribute("name") %>  
<%=request.getAttribute("age") %>  
<%=application.getAttribute("name") %>
```

## 应用案例

#### 一、 案例介绍

该例子是用于在用户 `完成登录后`，原本的登录按钮变为 **显示用户名** 的样式。

从而我们可以通过在 `后端` 向 **session** 中写入用户名，在 `前端` 中进行 **session** 的获取，进而检查 **session** 中是否含有 **用户名**，`如果含有`，那么就进行 **按钮名称的改变** 。

#### 二、 代码实现

 - `前端` 代码
 
```html
<%
	String div_style = "block";
	String is_online = "none";
	String again_login = "none";
	Object user = session.getAttribute("username");
	String username = (String) user;
	
	if (username != null) {
		div_style = "none";
		is_online = "block";
	}
%>

<div id="login" style="display: <%=div_style%>">
	<a href="userLogin">
		<button class="btn btn-success btn-lg">
		登录
		</button> 
	</a> 
	<a href="userRegister">
		<button class="btn btn-warning btn-lg">
		注册
		</button>
	</a>
</div>

<div id="login" style="display: <%=is_online%>">
	<a href="index.jsp">
		<button class="btn btn-success btn-lg">
			<%=username%>
		</button> 
	</a>
	<a href="user-exit">
		<button class="btn btn-warning btn-lg">
			退出登录
		</button> 
	</a>
</div>
```

 - `后端` 的实现代码
 
 - 用户`登录` 时

```java
public String login() {
    
    Map<String, Object> session = ActionContext.getContext().getSession();
    
	session = ActionContext.getContext().getSession();
	
	session.put("username", username);

	return SUCCESS;
}
```

 - 用户 `登出` 时

```java
// 用户登出
public String exit(){
	
	Map<String, Object> session = ActionContext.getContext().getSession();
	
	session.clear();
	
	// 测试时使用
	//System.out.println("111111");
	
	return "exit";
}
```

#### 三、 实现效果

 - **登录前** 页面的状态
 
<div align="center"><img src="http://www.hohott.wang/img/writing_img/2016-4-27/result1.png"/></div>

 - **登录后** 页面的状态

<div align="center"><img src="http://www.hohott.wang/img/writing_img/2016-4-27/result2.png"/></div>

就这样，我们实现了前后端的交互，页面上也呈现了我们期待的效果 : ）


  
## 著作权声明
本文首次发布于 [hohoTT's Blog](http://www.hohott.wang/)，转载请保留 [http://www.hohott.wang/2016/04/27/Struts-session/](http://www.hohott.wang/2016/04/27/Struts-session/) 以上链接并附上作者 **hohoTT**。如果您喜欢，还可以关注我的微信公众号 **hohoTT** , 或者扫一扫下方的微信公众号二维码即可 ^_^。
<div align="center"><img src="http://www.hohott.wang/img/WeiXinImg.jpg"/></div>

