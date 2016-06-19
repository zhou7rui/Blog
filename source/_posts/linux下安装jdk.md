title: linux下安装jdk
date: 2015-10-18 18:09:43
tags: Linux
---
在Linux下安装jdk: 介绍的是以安装gz压缩包为例
	Linux版本为：[Centos 7](https://www.centos.org/)
	Jdk版本为：[jdk7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)
<!-- more -->
### 1.首先下载jdk:
在Linux中使用wget的方式下载：wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie"  http://download.oracle.com/otn-pub/java/jdk/7u79-b15/jdk-7u79-linux-x64.tar.gz
**特别注意一定要加上前面的cookie在oracle官网下载要进行cookie处理**
以上桌面环境的略过

## 2.安装jdk:
```bash
ls
```
jdk-7u79-linux-x64.tar.gz

创建java 目录
```bash
 mkdir /usr/java
```

移动jdk到java目录下
```bash
mv -f jdk-7u79-linux-x64.tar.gz /usr/java
```
 切换到/usr/java下
 ``` bash
 cd /usr/java
 ```
 解压jdk
 ```bash
tar xvf jdk-7u79-linux-x64.tar.gz 
 ```
 查看jdk
 ```bash
 ls
 ```
 ![ls](/img/2015-10-18-ls.png)
 
 ### 3.设置jdk环境变量:
这里采用全局设置方法，就是修改etc/profile，它是是所有用户的共用的环境变量
```bash
vim /etc/profile
```
打开之后在末尾添加(**vim编辑器就不介绍了**)
```
export JAVA_HOME=/usr/java/jdk1.7.0_79
export JAVA_BIN=/usr/java/jdk1.7.0_79/bin
export PATH=$PATH:$JAVA_HOME/bin
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export JAVA_HOME JAVA_BIN PATH CLASSPATH
```
请记住，在上述添加过程中，等号两侧不要加入空格，不然会出现“不是有效的标识符”，因为source /etc/profile 时不能识别多余到空格，会理解为是路径一部分。
然后保存
使profile生效
```bash
source /etc/profile
```
执行
```bash 
java -version
```
**看到下图恭喜你安装成功**
 ![version](/img/2015-10-18-version.png)

