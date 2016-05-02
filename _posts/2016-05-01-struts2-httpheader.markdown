---
layout:     post
title:      "Struts2 中配置文件设置向前端返回的状态码"
subtitle:   " \"Struts2 配置文件应用\""
date:       2016-05-01 15:00:00
author:     "hohoTT"
header-img: "img/writing_img/2016-5-01/post-bg.png"
catalog: true
tags:
    - Java
    - Struts2
    - SSH
---

## 例子介绍

现在我在 `前端` 创建了一个用户注册界面，但是我要求 **用户名** 以及 **邮箱** 都 `不能被注册过` ，此时我们为了用户的交互方便，就需要使用 **AJAX** 的技术，实现 `异步` 的 **JavaScript** 操作。

因为有 **两个** 被要求的地方，所以此时，`后端` 同样应该有 **两个对应的API** 来与 `前端` 用户输入的信息进行交互。那么问题来了，当 `后端` 得到应有的响应处理后该向 `前端` 进行 **传递消息**，实现 **前后端** 的信息交互，我们此时 `后端` 应该返回这样的数据。

 - 一般情况下我们根据以下的方式处理

```java
context.Response.StatusCode = 400;
```

我们这个例子中用到的为 **SSH** 框架，那么我们应该怎么 `处理状态码` 的返回呢？

## 代码实现

#### 验证处理类

 - 首先创建 `后端` **验证处理类**

```java
package com.wt.jsonHandle;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.struts2.ServletActionContext;
import org.apache.struts2.interceptor.ServletResponseAware;

import com.opensymphony.xwork2.ActionContext;
import com.opensymphony.xwork2.ActionSupport;
import com.wt.entity.User;
import com.wt.service.UserService;

public class UserValidate extends ActionSupport{

	private static final long serialVersionUID = 1L;

	private UserService userService;
	
	ActionContext context = ActionContext.getContext();
	
	HttpServletRequest request = (HttpServletRequest)context.get(ServletActionContext.HTTP_REQUEST);
	
	public void setUserService(UserService userService) {
		this.userService = userService;
	}
	
	// 用户名验证
	public String usernameCheck(){
		
		String username = request.getParameter("username");
		
		// 验证用户名是否被注册使用
		User usernameCheckUser = userService.usernameCheck(username);
				
		// 如果用户名已被注册，返回该用户名已被注册！的错误消息
		if(usernameCheckUser != null){
			return "usernameError";
		}
		else{
			return "usernameSuccess";
		}
	}
	
	// 邮箱验证
	public String emailCheck(){
		
		String email = request.getParameter("email");
		
		// 验证邮箱是否被注册使用
		User emailCheckUser = userService.emailCheck(email);
				
		// 如果用户名已被注册，返回该用户名已被注册！的错误消息
		if(emailCheckUser != null){
			return "emailError";
		}
		else{
			return "emailSuccess";
		}
	}
	
}
```

关于 **usernameCheck** 以及 **emailCheck ** 两个方法，就是你通过 **hql** 语句进行 `数据库检查` 的操作方法，这里不做过多说明。

#### 声明验证处理类

 - 在 **applicationContext-beans.xml** 配置文件中进行 **验证处理类** 的 `声明` ,以下为声明代码
 
```java
<bean id="userValidate" class="com.wt.jsonHandle.UserValidate"
	  scope="prototype">
	  <property name="userService" ref="userService"></property>
</bean>
```

#### Struts.xml 配置文件

 - 最重要的一步，在 **Struts.xml 配置文件** 中对 `后端` 处理结果的 **状态码映射**，从而返回给 `前端` 正确的状态码，进而做出 `前端` 的消息显示

```java
<package name="default" namespace="/" extends="struts-default">
    <!-- 用户名验证 -->
	<action name="usernameCheck" class="userValidate"
			method="usernameCheck">
    	<result name="usernameSuccess" type="httpheader">  
	    	<param name="status">200</param>
	    </result>
	    
	    <result name="usernameError" type="httpheader">  
	    	<param name="status">400</param>
	    </result>
    </action>
    
    <!-- 邮箱验证 -->
	<action name="emailCheck" class="userValidate"
			method="emailCheck">
    	<result name="emailSuccess" type="httpheader">  
	    	<param name="status">200</param>
	    </result>
	    
	    <result name="emailError" type="httpheader">  
	    	<param name="status">400</param>
	    </result>
    </action>
	
</package>
```

代码清晰明了，**HttpHeader**（用来控制特殊的 **Http** 行为）， **httpheader** 结果类型很少使用到，它实际上是返回一个 **HTTP** 响应的头信息。

## 测试结果

 - 以下为 **数据库** 中的信息

<div align="center"><img src="http://www.hohott.wang/img/writing_img/2016-5-01/result1.png"/></div>

- 首先我们来测试 **用户名存在** 的情况

<div align="center"><img src="http://www.hohott.wang/img/writing_img/2016-5-01/result2.png"/></div>

 - 开发者工具，可以看到具体的信息，此时前端向后端进行了 **username** 为 `a` 的传递，进而后端返回的为状态码为 **400** 的响应

<div align="center"><img src="http://www.hohott.wang/img/writing_img/2016-5-01/result3.png"/></div>

- 同理，**邮箱** 的验证与 **用户名** 的验证如出一辙

<div align="center"><img src="http://www.hohott.wang/img/writing_img/2016-5-01/result4.png"/></div>

- 接下来，我们看下 **测试成功** 的情况

<div align="center"><img src="http://www.hohott.wang/img/writing_img/2016-5-01/success1.png"/></div>

 - 验证 **用户名** 成功时，后端返回状态码为 **200** 的响应给 `前端`
 
<div align="center"><img src="http://www.hohott.wang/img/writing_img/2016-5-01/success2.png"/></div>

 - 验证 **邮箱成功** 时，后端返回状态码为 **200** 的响应给 `前端`
 
<div align="center"><img src="http://www.hohott.wang/img/writing_img/2016-5-01/success3.png"/></div>
<br>
以上我们就利用 **Struts2**  给我们提供的设置，成功的完成了 `前后端` 的 **数据交互响应**  : )
   
## 著作权声明
本文首次发布于 [hohoTT's Blog](http://www.hohott.wang/)，转载请保留 [http://www.hohott.wang/2016/05/01/struts2-httpheader/](http://www.hohott.wang/2016/05/01/struts2-httpheader/) 以上链接并附上作者 **hohoTT**。如果您喜欢，还可以关注我的微信公众号 **hohoTT** , 或者扫一扫下方的微信公众号二维码即可 ^_^。
<div align="center"><img src="http://www.hohott.wang/img/WeiXinImg.jpg"/></div>