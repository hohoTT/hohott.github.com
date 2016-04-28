---
layout:     post
title:      "Struts2、Spring、Hibernate 框架整合搭建（一）下"
subtitle:   " \"SSH 框架整合搭建\""
date:       2016-04-26 21:00:00
author:     "hohoTT"
header-img: "img/post-bg-js-module.jpg"
catalog: true
tags:
    - Java
    - Eclipse
    - SSH
---

## 前景回顾

首先我们由之前的 [Struts2、Spring、Hibernate 框架介绍及框架的搭建](http://www.hohott.wang/2016/04/24/SSH-first/) 以及 [Struts2、Spring、Hibernate 框架整合搭建（一）上](http://www.hohott.wang/2016/04/25/SSH-build-first/) 两篇文章了解了 **SSH** 框架，同时我们最后实现了数据库表的创建。

那么我们接下来的工作，即为利用之前整合的 **SSH** 完成对 **数据库** 的相关操作，这里我们介绍的为 `查询数据库表的信息` 为例

## 功能及代码实现

 - 首先我们了解项目的 **目录结构**

<div align="center"><img src="http://www.hohott.wang/img/writing_img/2016-4-26/index.png"/></div>
 
#### 一、 实现数据库层的操作

 - 即我们 **com.wt.dao** 包下的类文件
 
 - 这里建议创建使用一个 `基类` ，便于 **简化** 代码，**减少** 代码冗余的问题，从而优化代码。
 
 - 创建 **BaseDao.java**

```java
package com.wt.dao;

import org.hibernate.Session;
import org.hibernate.SessionFactory;

public class BaseDao {
	
	private SessionFactory sessionFactory;
	
	public void setSessionFactory(SessionFactory sessionFactory) {
		this.sessionFactory = sessionFactory;
	}
	
	public Session getSession() {
		return this.sessionFactory.getCurrentSession();
	}

}
```

 - 创建对我们先前的用户表的 **数据库操作类**
 
 - 创建 **UserDao.java**
 
```java
package com.wt.dao;

import java.util.List;

import com.wt.entity.User;

public class UserDao extends BaseDao{

	public List<User> getAll(){
	
		String hql = "FROM User";
		
		return getSession().createQuery(hql).list();
		
	}
	
}
```

#### 二、 实现业务层的操作

 - 即 **com.wt.service** 包下的类
 - 创建对于用户表的 **业务逻辑处理** 的类

```java
package com.wt.service;

import java.util.List;

import com.wt.dao.UserDao;
import com.wt.entity.User;

public class UserService {

	private UserDao userDao;
	
	public void setUserDao(UserDao userDao) {
		this.userDao = userDao;
	}
	
	public List<User> getAll(){
		
		List<User> users = userDao.getAll();
		
		return users;
		
	}
	
}
```
 
#### 三、实现 Action

 - 这里我们把 **Action** 统一的放到 **com.wt.Action** 包下
 - 创建 **UserAction.java** 类

```java
package com.wt.action;

import java.util.List;
import java.util.Map;

import org.apache.struts2.interceptor.RequestAware;

import com.opensymphony.xwork2.ActionSupport;
import com.opensymphony.xwork2.ModelDriven;
import com.opensymphony.xwork2.Preparable;
import com.wt.entity.User;
import com.wt.service.UserService;

public class UserAction extends ActionSupport implements RequestAware,
ModelDriven<User>, Preparable{

	/**
	 * 
	 */
	private static final long serialVersionUID = 1L;
	
	private UserService userService;
	
	public void setUserService(UserService userService) {
		this.userService = userService;
	}
	
	public String list() {

		List<User> users = userService.getAll();
		
		// 以下为测试时使用
		for (User user : users) {
			
			System.out.println("User_name --- " + user.getUser_name());
			
		}
		
		return "list";
	}
	
	// login
	public String login(){
		
		String name = "";
		
		String password = "";
		
		return "login";
	}

	@Override
	public void prepare() throws Exception {}

	@Override
	public User getModel() {
		// TODO Auto-generated method stub
		return null;
	}

	@Override
	public void setRequest(Map<String, Object> arg0) {
		// TODO Auto-generated method stub
		
	}

}
```

 - 这里我们需要注意的实现的 **接口** ，这里是关于 **Struts2** 中的详解，之后会有博文整理 : )

## 代码整合 实现查询功能

 - 在我们的 **conf** 文件下创建 **applicationContext-beans.xml** 配置文件，用于向 **Spring** 框架中注册我们声明创建的 `数据库层`、`业务层`、以及到最后的 **Action处理类**
 
 - **applicationContext-beans.xml** 配置文件

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

	<bean id="userDao" class="com.wt.dao.UserDao">
		<property name="sessionFactory" ref="sessionFactory"></property>
	</bean>
	
	<bean id="userService" class="com.wt.service.UserService">
		<property name="userDao" ref="userDao"></property>
	</bean>

	<bean id="userAction" class="com.wt.action.UserAction"
		  scope="prototype">
		  <property name="userService" ref="userService"></property>
	</bean>

</beans>

```

 - 以上我们就完成了这个查询功能的 **SSH** 框架的所有整合

## 实现效果

 - 首先根据以下代码的实现要求 **控制台打印** `用户名`

<div align="center"><img src="http://www.hohott.wang/img/writing_img/2016-4-26/success2.png"/></div>

 - **控制台** 显示信息

<div align="center"><img src="http://www.hohott.wang/img/writing_img/2016-4-26/success1.png"/></div>

 - 对应数据库信息，证明查询用户表的 **功能实现**

<div align="center"><img src="http://www.hohott.wang/img/writing_img/2016-4-26/success3.png"/></div>
 
至此关于我们的 **SSH** 第一部分的整合以及功能的实现结束 : )
 
 
## 著作权声明
本文首次发布于 [hohoTT's Blog](http://www.hohott.wang/)，转载请保留 [http://www.hohott.wang/2016/04/26/SSH-build-second/](http://www.hohott.wang/2016/04/26/SSH-build-second/) 以上链接并附上作者 **hohoTT**。如果您喜欢，还可以关注我的微信公众号 **hohoTT** , 或者扫一扫下方的微信公众号二维码即可 ^_^。
<div align="center"><img src="http://www.hohott.wang/img/WeiXinImg.jpg"/></div>