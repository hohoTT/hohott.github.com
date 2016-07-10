---
layout:     post
title:      "Java Maven 相关知识点的总结"
subtitle:   " \"Java Maven\""
date:       2016-6-01 11:00:00
author:     "hohoTT"
header-img: "img/writing_img/2016-6-01/post-bg.jpg"
catalog: true
tags:
    - Java
    - Web
    - Maven
---

## Maven 介绍

**Apache Maven**，是一个软件（特别是 **Java** 软件）`项目管理` 及 `自动构建` 工具，由Apache软件基金会所提供。基于项目对象模型（缩写：**POM**）概念，**Maven** 利用一个中央信息片断能管理一个项目的 `构建`、`报告` 和 `文档` 等步骤。


**Maven** 也可被用于 `构建` 和 `管理` 各种项目，例如 **C#，Ruby，Scala** 和其他语言编写的项目。**Maven** 曾是 **Jakarta** 项目的子项目，现为由 **Apache** 软件基金会主持的独立 **Apache项目**。

## Maven 项目的目录结构

    src 
    	-main
    		-java
    			-package
    	-test
    		-java
    			-package
    	resources
    	
    	
    groupId 就是项目的包名
    artifactId 是模块名

按照这个结构进行 **Java Maven** 项目的搭建，对初始化创建的 `不完整` 的 **Maven结构** 按照上面的结构进行补全

## Maven 中常用到的命令

#### 命令介绍

    mvn -v				查看 maven 版本
    	compile			编译
    	test			测试
    	package			打包
    	
    	clean			删除 target
    	install 		安装 jar 包到本地仓库中
    	
#### 创建目录的两种方式
    	
`创建目录` 的两种方式：

1. archetype:generate	

自动安装搭建 **maven** 的项目结构骨架，但是之后要输入一些有关 **pom.xml** 中的内容
		
2. archetype:generate 

-DgroupId=com.wt.mavenAutoCreate 	`组织名，公司网址的反写+项目名`

-DartifactId=mavenAutoCreate-service  `项目名-模块名`

-Dversion=1.0.0-SNAPSHOT 			  `版本号`

-Dpackage=com.wt.mavenAutoCreate	  `代码所存在的包`
	
#### archetype 插件

	用于创建符合 maven 规定的目录结构 
	
	坐标
		构件
	
	仓库
		本地仓库和远程仓库
		
	镜像仓库
	
配置 `-Dmaven.multiModuleProjectDirectory=$M2_HOME`  注意此时的 **JDK** 版本需要设置为 `1.7` 的版本，`1.8` 的版本会报错 

## Maven 中依赖的介绍

依赖的第一部分讲解：
**maven-a、maven-b、maven-c**  三个项目的一开关系为：

1. `maven-a` 依赖于 `maven-b`
2. `maven-c` 依赖于 `maven-a`

所以  `maven-c` 依赖于 `maven-b`

依赖的第二部分讲解：（越接近依赖与哪一个）

c - >  a - > b

2.0   2.0   2.4

`maven-b` 中使用的版本为 2.4

`maven-a` 中使用的版本为 2.0

因为 `maven-c` 离 `maven-a` 近，所以版本为2.0

`maven-c` 中使用的版本为 2.0

依赖的第三部分讲解：（距离相同时，依赖与加载的顺序，先加载先依赖）

依赖的继承：
实现 **maven项目** 中的依赖继承关系，创建一个 **父maven项目** **maven-parent**，使得 **maven-b** 继承与 **maven-parent**，实现代码的复用

## Maven 仓库位置修改及配置文件的相关设置

找到 **settings.xml** 文件，添加以下的配置即可
以下的操作均为对 **settings.xml** 文件的相关操作

#### 仓库位置的修改

```xml
<localRepository>F:/Maven/repo</localRepository>
```

#### 修改 maven web 工程默认的jdk版本

```xml
<profile>
	
	  <id>jdk-1.7</id>

	  <activation>

			 <activeByDefault>true</activeByDefault>

			 <jdk>1.7</jdk>

	  </activation>

	  <properties>

			 <maven.compiler.source>1.7</maven.compiler.source>

			 <maven.compiler.target>1.7</maven.compiler.target>

			 <maven.compiler.compilerVersion>1.7</maven.compiler.compilerVersion>

	  </properties>

</profile>
```

#### maven 配置文件中修改仓库的位置，修改为英国的仓库

```xml
<mirror>

  <id>UK</id>

  <name>UK Central</name>

  <url>http://uk.maven.org/maven2</url>

  <mirrorOf>central</mirrorOf>

</mirror>
```


## Maven 项目配置 Tomcat 服务器

共有 `两种方式`，均属于 **pom.xml** 文件的配置

#### 第一种方式

```xml
<build>
	<finalName>maven-webDemo</finalName>

	<plugins>
		<!-- 以下为嵌入Tomcat服务器   -->
		<plugin>
			<groupId>org.apache.tomcat.maven</groupId>
			<artifactId>tomcat7-maven-plugin</artifactId>
			<version>2.1</version>
			<configuration>
				<port>8080</port>
				<path>/maven-webDemo</path>
				<uriEncoding>UTF-8</uriEncoding>
				<finalName>maven-webDemo</finalName>
				<server>tomcat7</server>
			</configuration>
			
			<!-- 以下的配置为在打包成功后使用tomcat:run来运行tomcat服务 -->
			<executions>
				<execution>
					<phase>package</phase>
					<goals>
						<goal>run</goal>
					</goals>
				</execution>
			</executions>
		</plugin>

	</plugins>
</build>
```

#### 第二种方式

```xml
<build>
	<finalName>maven-webDemo</finalName>

	<plugins>

		<!-- 以下为嵌入Tomcat服务器 -->
		<plugin>
			<groupId>org.apache.tomcat.maven</groupId>
			<artifactId>tomcat7-maven-plugin</artifactId>
			<version>2.2</version>

			<!-- 以下的配置为在打包成功后使用tomcat:run来运行tomcat服务 -->
			<executions>
				<execution>
					<phase>package</phase>
					<goals>
						<goal>run</goal>
					</goals>
				</execution>
			</executions>
		</plugin>

	</plugins>
</build>
```

**推荐使用** `第二种方式` 进行 **Maven** 项目中的 **Tomcat 服务器** 配置

配置结束之后，**maven build** 时直接输入命令 **clean package** 即可: )


## 著作权声明
本文首次发布于 [hohoTT's Blog](http://www.hohott.wang/)，转载请保留 [http://www.hohott.wang/2016/06/01/Java-Maven-Summary/](http://www.hohott.wang/2016/06/01/Java-Maven-Summary/) 以上链接并附上作者 **hohoTT**。如果您喜欢，还可以关注我的微信公众号 **hohoTT** , 或者扫一扫下方的微信公众号二维码即可 ^_^。
<div align="center"><img src="http://www.hohott.wang/img/WeiXinImg.jpg"/></div>




