---
title: sprin注释bean名头字母大小写的问题
date: 2016-06-19 16:37:40
tags: java
---
在最近的工作中碰到spring注解注入bean的name出错的情况,当时使用@Controller，,@Service,@Repository,注解时在没有指定value的值时默认会将类名首字母小写，
``` java
  @Repository
  public interface ITestDao{


  }
```
<!-- more -->
按着这个思路在service层按名称注入dao时
``` java
  @Service
  public class TestService{

    @Resource("iTestDao")
    private ITestDao iTestDao;
  }
```
当程序运行起来报错找不到bean,找了很久才发现：改为
``` java
  @Service
  public class TestService{

    @Resource("ITestDao")
    private ITestDao iTestDao;
  }
```
则运行成功。当类名类名前两个字母都是大写，则返回的bean名也是大写的即类名为ITestDao,bean名也是ITestDao.
