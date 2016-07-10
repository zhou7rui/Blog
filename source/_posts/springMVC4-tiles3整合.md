---
title: springMVC4+tile3整合
date: 2016-07-10 22:26:04
tags: java
---
# tiles是什么：
Tiles框架是一个页面模版引擎，用来渲染页面，属于视图层。Tiles框架使用模板定义网页布局，每个页面模板都是一个简单的 JSP 页面。Tiles模板（Layout）是一种描述页面布局的JSP页面，Tiles模板只定义了Web页面的样式，而不指定里面的内容。当Web页面运行的时侯，才把特定的JSP内容插入到模板中显示。Tiles 框架建立在JSP的include指令的基础上，但它提供了比JSP的 include指令更强大的功能。
 <!-- more -->
项目结构：<br>
![项目结构图](/img/tiles-1.png)
## 一.pom.xml的更新
需要加入的jar包。
``` xml
<properties>
        <springframework.version>4.2.6.RELEASE</springframework.version>
        <apachetiles.version>3.0.5</apachetiles.version>
    </properties>

    <dependencies>
        <!-- Spring 的jar包 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${springframework.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>${springframework.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${springframework.version}</version>
        </dependency>
        <!-- Apache Tiles 的相关jar包-->
        <dependency>
            <groupId>org.apache.tiles</groupId>
            <artifactId>tiles-core</artifactId>
            <version>${apachetiles.version}</version>
        </dependency>
        <dependency>
            <groupId>org.apache.tiles</groupId>
            <artifactId>tiles-api</artifactId>
            <version>${apachetiles.version}</version>
        </dependency>
        <dependency>
            <groupId>org.apache.tiles</groupId>
            <artifactId>tiles-servlet</artifactId>
            <version>${apachetiles.version}</version>
        </dependency>
        <dependency>
            <groupId>org.apache.tiles</groupId>
            <artifactId>tiles-jsp</artifactId>
            <version>${apachetiles.version}</version>
        </dependency>

        <!-- Servlet+JSP+JSTL -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.1.0</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>javax.servlet.jsp-api</artifactId>
            <version>2.3.1</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>

    </dependencies>
```
这里除了spring mvc的jar包还加入了Apache-tiles3的jar包

## 二tiles配置文件的配置
创建Layout.xml文件名字随便取都行.
``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE tiles-definitions PUBLIC  "-//Apache Software Foundation//DTD Tiles Configuration 3.0//EN"  "http://tiles.apache.org/dtds/tiles-config_3_0.dtd">

<tiles-definitions>
  <!-- Base index -->
  <definition name="index"
              template="/WEB-INF/views/index.jsp">
      <put-attribute name="title" value="" />
      <put-attribute name="header" value="/WEB-INF/views/header.jsp" />
      <put-attribute name="menu" value="/WEB-INF/views/menu.jsp" />
      <put-attribute name="body" value="" />
      <put-attribute name="footer" value="/WEB-INF/views/header.jsp" />
  </definition>


</tiles-definitions>
```
依次在/WEB-INF/views/目录下创建index.jsp,header.jsp,menu.jsp,header.jsp文件
![jsp结构文件](/img/tiles-2.png)
### index.jsp
``` jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%--导入tiles文件 --%>
<%@taglib prefix="tiles" uri="http://tiles.apache.org/tags-tiles" %>
<html>
<head>
  <title>index</title>
</head>
<body>
      <header id="header">
          <tiles:insertAttribute name="header" />
      </header>

      <section id="sidemenu">
          <tiles:insertAttribute name="menu" />
      </section>

      <section id="site-content">
          <tiles:insertAttribute name="body" />
      </section>

      <footer id="footer">
          <tiles:insertAttribute name="footer" />
      </footer>
</body>
</html>
```
### header.jsp
``` xml
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <H1>header</H1>
</body>
</html>
```
### menu.jsp
``` xml
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>menu</title>
</head>
<body>
    <h1>menu</h1>
</body>
</html>

```
### header.jsp
``` xml
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>header</title>
</head>
<body>
    <h1>header</h1>
</body>
</html>
```
## 三.springMVC与Tile的配置
``` xml

<!-- 注解扫描位置 -->
  <!-- springMVC其他配置省略.....  -->
  <!--加载tiles配置文件 -->
 <bean id="tilesConfigurer" class="org.springframework.web.servlet.view.tiles3.TilesConfigurer">
     <property name="definitions" value="WEB-INF/layout.xml" />
 </bean>
 <!--springmvc 视图渲染配置-->
 <bean id="viewResolver" class="org.springframework.web.servlet.view.UrlBasedViewResolver">
    <!-- 设置优先级-->
     <property name="order" value="1" />
     <property name="viewClass" value="org.springframework.web.servlet.view.tiles3.TilesView"></property>
 </bean>

 <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <!-- 设置优先级-->
     <property name="order" value="2"></property>
     <property name="prefix" value="/WEB-INF/views/"></property>
     <property name="suffix" value=".jsp"></property>

 </bean>

```
启动项目看到
![结果](/img/tiles-3.png)
