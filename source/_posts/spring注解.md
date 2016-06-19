title: spring注解
date: 2015-10-07 20:59:24
tags: java
---
## 组件扫描：
在spring配置文件中加入需要扫描的包
```xml
<context:component-scan base-package=”com.mmnc”>
```
1、@controller 控制器（注入服务）
2、@service 服务（注入dao）
3、@repository dao（实现dao访问）
4、@component （把普通pojo实例化到spring容器中，相当于配置文件中的<bean id="" class=""/>）
@Component,@Service,@Controller,@Repository注解的类，并把这些类纳入进spring容器中管理。
<!-- more -->   
## 下面写这个是引入component的扫描组件


其中base-package为需要扫描的包（含所有子包）
       1、@Service用于标注业务层组件
       2、@Controller用于标注控制层组件(如struts中的action)
       3、@Repository用于标注数据访问组件，即DAO组件.
       4、@Component泛指组件，当组件不好归类的时候，我们可以使用这个注解进行标注。    
          @Service public class UserServiceImpl implements UserService { }
          @Repository public class UserDaoImpl implements UserDao { } getBean的默认名称是类名（头字母小写），如果想自定义，可以@Service("***") 这样来指定，这种bean默认是单例的，如果想改变，可以使用@Service(“beanName”) 
          @Scope(“prototype”)来改变。可以使用以下方式指定初始化方法和销毁方法（方法名任意）： @PostConstruct public void init() { }
## 组件自动装配：
  Spring不但支持自己定义的@Autowired注解，还支持几个由JSR-250规范定义的注解，它们分别是@Resource、@PostConstruct以及@PreDestroy。
　@Resource的作用相当于@Autowired，只不过@Autowired按byType自动注入，而@Resource默认按 byName自动注入罢了。@Resource有两个属性是比较重要的，分是name和type，Spring将@Resource注解的name属性解析为bean的名字，而type属性则解析为bean的类型。所以如果使用name属性，则使用byName的自动注入策略，而使用type属性时则使用byType自动注入策略。如果既不指定name也不指定type属性，这时将通过反射机制使用byName自动注入策略。
　@Resource装配顺序
　1. 如果同时指定了name和type，则从Spring上下文中找到唯一匹配的bean进行装配，找不到则抛出异常
　2. 如果指定了name，则从上下文中查找名称（id）匹配的bean进行装配，找不到则抛出异常
　3. 如果指定了type，则从上下文中找到类型匹配的唯一bean进行装配，找不到或者找到多个，都会抛出异常
　4. 如果既没有指定name，又没有指定type，则自动按照byName方式进行装配；如果没有匹配，则回退为一个原始类型进行匹配，如果匹配则自动装配；
@Autowired 与@Resource的区别：
 1、 @Autowired与@Resource都可以用来装配bean. 都可以写在字段上,或写在setter方法上。
 2、 @Autowired默认按类型装配（这个注解是属业spring的），默认情况下必须要求依赖对象必须存在，如果要允许null值，可以设置它的required属性为false，如：@Autowired(required=false) ，如果我们想使用名称装配可以结合@Qualifier注解进行使用，如下：

```java
@Autowired()
@Qualifier("baseDao")
private BaseDao baseDao;
```


3、@Resource（这个注解属于J2EE的），默认安装名称进行装配，名称可以通过name属性进行指定，如果没有指定name属性，当注解写在字段上时，默认取字段名进行安装名称查找，如果注解写在setter方法上默认取属性名进行装配。当找不到与名称匹配的bean时才按照类型进行装配。但是需要注意的是，如果name属性一旦指定，就只会按照名称进行装配。
```java
@Resource(name="baseDao")
private BaseDao baseDao;
```



**推荐使用：@Resource注解在字段上，这样就不用写setter方法了，并且这个注解是属于J2EE的，减少了与spring的耦合。这样代码看起就比较优雅。**
