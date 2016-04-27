---
layout:     post
title:      "Struts2、Spring、Hibernate 框架整合搭建（一）上"
subtitle:   " \"SSH 框架整合搭建\""
date:       2016-04-25 14:00:00
author:     "hohoTT"
header-img: "img/writing_img/2016-4-25/post-bg.png"
catalog: true
tags:
    - Java
    - Eclipse
    - SSH
---

## 背景介绍

由之前 [Struts2、Spring、Hibernate 框架介绍及框架的搭建](http://www.hohott.wang/2016/04/24/SSH-first/) 文章中我们已经对 **SSH** 框架有了初步的了解，那么接下了我们进行有关 **SSH框架** 的搭建过程。


## 项目目标

以下介绍的是项目需要达到的任务目标：

* 配置 **Spring** 文件，

* 使用 **Hibernate** 自动生成数据库表
    * 根据之前博文中的介绍，配置 **Spring** 文件，项目将会自动创建 **sessionFactory** 工厂，从而会自动的生成数据库表，
    * `当然这里要注意的是，你需要手动先创建一个项目数据库表存放的数据库`， 没有这个数据库是万万不行的，那样会使项目数据库表没有存放位置。

* 加入 **Struts2**


## 项目流程

以下介绍的是项目建立的具体实施步骤：

 1. 加入 **Spring**
		<1>. 加入需要的 **jar** 包
		<2>. 配置 **web.xml** 文件
		<3>. 加入 **Spring** 的配置文件

 2. 加入 **Hibernate**
    <1>. 建立持久化类和对应的 **.hbm.xml** 文件 ，生成对应的数据表
    <2>. **Spring** 整合 **Hibernate**
    <3>. 步骤

        ① 加入 jar 包
        ② 在类路径下加入 hibernate.cfg.xml 文件，在其中配置 hibernate 的基本属性
        ③ 建立持久化类，和其对应的 .hbm.xml 文件
        ④ 和 Spring 进行整合
            i.  加入 c3p0 和 MySQL 的驱动
            ii. 在 Spring 的配置文件中配置: 数据源, SessionFactory, 声明式事务
        ⑤ 启动项目, 会看到生成对应的数据表

 3. 加入 **Struts2**

    <1>. 加入 **jar** 包: 若有 `重复` 的 **jar** 包, 则需要删除 `版本较低` 的. **javassist-3.11.0.GA.jar**
    <2>. 在 **web.xml** 文件中配置 **Struts2** 的 **Filter**
	<3>. 加入 **Struts2** 的配置文件
	<4>. 整合 **Spring**

        ① 加入 Struts2 的 Spring 插件的 jar 包
        ② 在 Spring 的配置文件中正常配置 Action, 注意 Action 的 scope 为 prototype
        ③ 在 Struts2 的配置文件中配置 Action 时, class 属性指向该 Action 在 IOC 中的 id

 4. 完成任务
    获取用户表的数据

## 代码实现

#### 一、 项目创建初始化

首先创建一个 `动态` 的项目工程
<div align="center"><img src="http://www.hohott.wang/img/writing_img/2016-4-25/create.png"/></div>

添加项目工程中需要的 **jar** 包
<div align="center"><img src="http://www.hohott.wang/img/writing_img/2016-4-25/jar.png"/></div>

因为项目中需要的配置文件信息比较多，所以创建conf资源文件以便管理配置文件，以下为项目工程的目录结构图
<div align="center"><img src="http://www.hohott.wang/img/writing_img/2016-4-25/index.png"/></div>

#### 二、 Spring 部分

 - 配置 **web.xml** 文件, 即为 **Spring** 在项目 `服务器容器` 中进行声明

```java
<context-param>
	<param-name>contextConfigLocation</param-name>
	<param-value>classpath:applicationContext*.xml</param-value>
</context-param>

<listener>
	<listener-class>
		org.springframework.web.context.ContextLoaderListener
	</listener-class>
</listener>
```

 - 在 **conf** 目录下创建 **db.properties** 文件，配置 **C3P0 数据源** 部分

```java
jdbc.user=... //(你自己的用户名)
jdbc.password=...//(你自己的用户名密码)
jdbc.driverClass=com.mysql.jdbc.Driver
jdbc.jdbcUrl=jdbc:mysql://localhost:3306/Logistics

jdbc.initPoolSize=5
jdbc.maxPoolSize=10
#...
```

 - 创建 **Spring** 的配置文件
 <div align="center"><img src="http://www.hohott.wang/img/writing_img/2016-4-25/spring1.png"/></div>

 - 配置文件 **applicationContext.xml**

<div align="center"><img src="http://www.hohott.wang/img/writing_img/2016-4-25/spring2.png"/></div>


 - **Spring** 的配置文件
    - 需要注意这里用到的资源文件以及配置 **C3P0 数据源** 部分
    - 注意这其中加入了 **Spring** 中 **sessionFactory** 以及 声明式事务的配置信息
    - 如果只需要达到项目运行只 `创建数据库表` 的情况，只需要配置 **SessionFactory** 即可达到目的
    - 声明式事务的配置在实现对数据表具体操作时用到，该项目实现的查询数据库表的功能

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
xmlns:context="http://www.springframework.org/schema/context" xmlns:tx="http://www.springframework.org/schema/tx"
xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.0.xsd
http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd
http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.0.xsd">

	<!-- 导入资源文件 -->
	<context:property-placeholder location="classpath:db.properties" />

	<!-- 配置 C3P0 数据源 -->
	<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
		<property name="user" value="${jdbc.user}"></property>
		<property name="password" value="${jdbc.password}"></property>
		<property name="driverClass" value="${jdbc.driverClass}"></property>
		<property name="jdbcUrl" value="${jdbc.jdbcUrl}"></property>

		<property name="initialPoolSize" value="${jdbc.initPoolSize}"></property>
		<property name="maxPoolSize" value="${jdbc.maxPoolSize}"></property>
	</bean>

	<!-- 配置 SessionFactory -->
	<bean id="sessionFactory"
		class="org.springframework.orm.hibernate4.LocalSessionFactoryBean">
		<property name="dataSource" ref="dataSource"></property>
		<property name="configLocation" value="classpath:hibernate.cfg.xml"></property>
		<property name="mappingLocations" value="classpath:com/wt/entity/*.hbm.xml"></property>
	</bean>

	<!-- 配置 Spring 的声明式事务 -->
	<!-- 1. 配置 hibernate 的事务管理器 -->
	<bean id="transactionManager"
		class="org.springframework.orm.hibernate4.HibernateTransactionManager">
		<property name="sessionFactory" ref="sessionFactory"></property>
	</bean>

	<!-- 2. 配置事务属性 -->
	<tx:advice id="txAdvice" transaction-manager="transactionManager">
		<tx:attributes>
			<tx:method name="get*" read-only="true" />
			<tx:method name="lastNameIsValid" read-only="true" />
			<tx:method name="*" />
		</tx:attributes>
	</tx:advice>

	<!-- 3. 配置事务切入点, 再把事务属性和事务切入点关联起来 -->
	<aop:config>
		<aop:pointcut expression="execution(* com.wt.service.*.*(..))"
			id="txPointcut" />
		<aop:advisor advice-ref="txAdvice" pointcut-ref="txPointcut" />
	</aop:config>

</beans>
```

#### 三、 Hibernate 部分

 - 创建 **Hibernate** 的配置文件

```java
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-configuration PUBLIC
		"-//Hibernate/Hibernate Configuration DTD 3.0//EN"
		"http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
    <session-factory>

    	<!-- 配置 hibernate 的基本属性 -->

    	<!-- 方言 -->
		<property name="dialect">org.hibernate.dialect.MySQLDialect</property>

    	<!-- 是否显示及格式化 SQL -->
    	<property name="hibernate.show_sql">true</property>
    	<property name="hibernate.format_sql">true</property>

    	<!-- 生成数据表的策略 -->
    	<property name="hibernate.hbm2ddl.auto">update</property>

    	<!-- 二级缓存相关 -->

    </session-factory>
</hibernate-configuration>
```

 - 建立 **持久化类**

```java
package com.wt.entity;

public class User {
	private Integer user_id;
	private String user_name;
	private String user_password;

public Integer getUser_id() {
	return user_id;
}

public void setUser_id(Integer user_id) {
	this.user_id = user_id;
}

public String getUser_name() {
	return user_name;
}

public void setUser_name(String user_name) {
	this.user_name = user_name;
}

public String getUser_password() {
	return user_password;
}

public void setUser_password(String user_password) {
	this.user_password = user_password;
}

public User(Integer user_id, String user_name, String user_password) {
	super();
	this.user_id = user_id;
	this.user_name = user_name;
	this.user_password = user_password;
}

public User() {
	// TODO Auto-generated constructor stub
}

@Override
public String toString() {
	return "User [user_id=" + user_id + ", user_name=" + user_name
			+ ", user_password=" + user_password + "]";
}
}
```

 - 对应的 **.hbm.xml** 文件

```java
<?xml version="1.0"?>
<!DOCTYPE hibernate-mapping PUBLIC "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
"http://hibernate.sourceforge.net/hibernate-mapping-3.0.dtd">
<hibernate-mapping>

    <class name="com.wt.entity.User" table="USER">

        <id name="user_id" type="java.lang.Integer">
            <column name="USER_ID" />
            <generator class="native" />
        </id>

        <property name="user_name" type="java.lang.String">
            <column name="USER_NAME" />
        </property>

        <property name="user_password" type="java.lang.String">
            <column name="USER_PASSWORD" />
        </property>

    </class>

</hibernate-mapping>
```

 - 完成以上三步，运行项目，测试发现数据库表已 `自动创建` 并生成
 -


## 著作权声明
本文首次发布于 [hohoTT's Blog](http://www.hohott.wang/)，转载请保留 [http://www.hohott.wang/2016/04/25/SSH-build-first/](http://www.hohott.wang/2016/04/25/SSH-build-first/) 以上链接并附上作者 **hohoTT**。如果您喜欢，还可以关注我的微信公众号 **hohoTT** , 或者扫一扫下方的微信公众号二维码即可 ^_^。
<div align="center"><img src="http://www.hohott.wang/img/WeiXinImg.jpg"/></div>
