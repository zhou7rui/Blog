layout: w
title: centos7下MySQL安装
date: 2015-10-07 21:15:25
tags: Linux
---
 CentOS 7的yum源中貌似没有正常安装mysql时的mysql-sever文件，需要去官网上下载
 ``` bash
# wget http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm
# rpm -ivh mysql-community-release-el7-5.noarch.rpm
# yum install mysql-community-server
```
成功安装之后重启mysql服务

<!-- more -->

``` bash
# service mysqld restart
```
初次安装mysql是root账户是没有密码的
设置密码的方法
``` bash
# mysql -uroot
mysql> set password for ‘root’@‘localhost’ = password('mypasswd');
mysql> exit
```