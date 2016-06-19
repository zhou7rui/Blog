title: linux下安装tomcat
date: 2015-10-18 19:19:13
tags: Linux
---
### 安装说明 
安装环境：CentOS-7
安装方式：源码安装 
软件：apache-tomcat-7.0.64.tar
下载地址：http://tomcat.apache.org/download-70.cgi
<!-- more -->

### 安装前提 
系统必须已安装配置JDK6+，安装请参考：[在CentOS 7中安装与配置JDK8。] (http://zruihao68.github.io/2015/10/18/linux下安装jdk/)

### 安装tomcat 
将apache-tomcat-7.0.64.tar文件上传到/usr/java中执行以下操作： 
```bash
 cd /usr/java  
 tar xvf  apache-tomcat-7.0.64.tar # 解压压缩包  
 rm -rf apache-tomcat-7.0.64.tar #删除压缩包  
```
### 启动TOMCAT
执行以下操作：
```bash
cd cd apache-tomcat-7.0.64/bin  #切换到bin下
startup.sh              #启动
```
显示如下，则代表成功
Using CATALINA_BASE:   /usr/java/apache-tomcat-7.0.64
Using CATALINA_HOME:   /usr/java/apache-tomcat-7.0.64
Using CATALINA_TMPDIR: /usr/java/apache-tomcat-7.0.64/temp
Using JRE_HOME:        /usr/java/jdk1.7.0_79/jre
Using CLASSPATH:       /usr/java/apache-tomcat-7.0.64/bin/bootstrap.jar:/usr/java/apache-tomcat-7.0.64/bin/tomcat-juli.jar
Tomcat started.
检验Tomcat安装运行
通过以下地址查看tomcat是否运行正常：
http://127.0.0.1:8080/
看到tomcat系统界面，说明安装成功！
### 停止Tomcat
```bash
shutdown.sh #停止tomcat  
```