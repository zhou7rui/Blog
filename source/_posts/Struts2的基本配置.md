title: Struts2的基本配置
date: 2015-10-07 17:04:35
tags: java
---
## Struts2框架的配置
 1.导入.[jar包](http://struts.apache.org/download.cgi#struts23241)
 2.配置web.xml:拦截器
``` XML
	<filter>
        <filter-name>struts2</filter-name>
        <filter-class>org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter</filter-class>
    </filter>

    <filter-mapping>
        <filter-name>struts2</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```
<!-- more -->
3.配置struts.xml
       package:包，struts2使用package 来组织模块
       name 属性：必须，用于其它的包应用当前包
       extends:当前包继承那个包，继承的，既可以继承其中的所有的配置，通常情况下继承steuts-default
       namespace:struts-default在struts-default.xml中文件中定义 namespace 可选,如果没有给出，则以"/"为默认值，若namespace有一非默认值，则要想调用这个包里的 Action,就必须把这个属性所定义的命名空间添加到相关的URL字符串里Http://localhost:8080/contextPath/namespace/actionName.action

``` XML 
<package name="helloworld" extends="struts-default" >
   <!-- 配置一个action:一个struts的请求就是一个action 
        name:对应一个struts2的请求的名字(或者servltetPath,但去除出/和拓展名)，不包含拓展名
        class 的默认值是：com.opensymphony.xwork2.ActionSupport
        method 的默认值为：execute
        result:结果。
    -->
<action name="product-input">
    <!-- 
       result: 结果.表示action方法执行后可能返回的一个结果。所以一个action 节点可能会有多个result子节点多个result字节点使用name来区别
       name:  标识一个result.和action 方法返回。默认success
       type:  表示结果的类型，默认值为dispatcher(转发结果)
     -->
        <result>/WEB-INF/pages/input.jsp</result>
    </action>
</package>
```