title: java注解
date: 2015-10-07 22:26:42
tags: java
---
# 注解(也称元数据) 注解目前非常的流行，很多主流框架都支持注解，使我们可以稍后的某个时刻非常方便地使用这些数据。
   &nbsp;&nbsp;注解是众多引入到Java SE5中的重要的语言变化之一。注解可以使得我们能够以将由编译器来测试和验证的格式，储存有关程序的
   额外信息。注解可以用来生成描述文件，甚至或是新的类定义，并且有助于减轻编写"样板"代码的负担。通过使用注解，我们可以
   将这些元数据保存在Java源码中，并利用annotation API 为自己的注解造处理工具，同时，注解的优点还包括：更加干净的易读的
   <!-- more -->
   代码以及编译器类型检查等。虽然JDK中预先定义了一些元数据，但一般来说主要还是需要自己添加新的注解，并且按自己的方式使用它们。
### 注解语法：

  注解的语法比较简单，除了使用@之外，他基本语java固有的语法一致。java.lang中的注解：
  * @Override,表示当前的方法将覆盖超类中的方法。
  * @Deprecated,使用为它的元素，那么编译器会发出警告信息。
  * @SuppressWarnings,关闭不当的警告信息

### 接下来开始自己添加新的注解
首先先了解一下元注解， 元注解就是专职负责注解其他的注解：
![元注解](/img/zhujie.png)
&nbsp;&nbsp;定义一个注解的方式：
``` java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface Test { 
 }
```

除了@符号，注解很像是一个接口。定义注解的时候需要用到元注解，上面用到了@Target和@RetentionPolicy，它们的含义在上面的表格中已经标注。
在注解中一般会有一些元素以表示某些值。注解的元素看起来就像接口的方法，唯一的区别在于可以为其制定默认值。没有元素的注解称为标记注解，上面的@Test就是一个标记注解。 
** 注解的可用的类型包括以下几种：所有基本类型、String、Class、enum、Annotation、以上类型的数组形式。元素不能有不确定的值，即要么有默认值，要么在使用注解的时候提供元素的值。而且元素不能使用null作为默认值。注解在只有一个元素且该元素的名称是value的情况下，在使用注解的时候可以省略“value=”，直接写需要的值即可。**
  下面看一个定义了元素的注解。 
``` java
package com.zrui.annotation;
import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Inherited;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
@Target(ElementType.METHOD)                   //设置注解的位置(方法上面还是类接口上面)
@Retention(RetentionPolicy.RUNTIME)			 //注解的作用域
@Inherited								    //表示可继承
@Documented
public @interface Table {
	String value();
	

}
```
定义了注解就去使用它
``` java 
package com.zrui.annotation;
@Table("user")
public class User {
	private Integer id;
	private String userName;
	........
	
}
```
###  注解的处理：
注解不处理，注解可以说是没有实用价值
使用注解最主要的部分在于对注解的处理，那么就会涉及到注解处理器。
 从原理上讲，注解处理器就是通过反射机制获取被检查方法上的注解信息，然后根据注解元素的值进行特定的处理。
``` java
	public class TestMain {
	public static void main(String[] args) {
		//使用类加载器加载类
	    try {
			Class<?>  c=Class.forName("com.zrui.annotation.User");
		//找到类上面的注解
		 boolean  isExist =	c.isAnnotationPresent(Table.class);
		 if(isExist){
			 //拿到注解实例
			 Table d = (Table) c.getAnnotation(Table.class);
			 System.out.println(d.value());
		 }
		 //找到方法上的注解
		/* Method[] md =  c.getMethods();
		 for (Method me : md) {
			 boolean  isMxist = me.isAnnotationPresent(Table.class);
			 if(isMxist){
				 Table  d1 = me.getAnnotation(Table.class);
				 System.out.println(d1.value());
			 }
		 }*/
		 //另一种解析方式
		 for (Method me : md) {
			 Annotation[] as = me.getAnnotations();
			 for (Annotation an : as) {
				 if (an instanceof Table) {
					 Table de = (Table)an;
					System.out.println(de.value());
				}
			}
		}	 
		} catch (ClassNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
}
```
上面两种方式都可以打印出value：user

通过上面的例子得到表的名字 如果再加以处理拼装下sql语句 是否可以完成一个query()方法自动的生成sql 一个简化版的hibernate

### 总结
** 在这篇文档中，我们解释了Java注解是Java1.5开始一个非常重要的特性。基本上，注解都是作为包含代码信息的元数据而被标记到代码中。它们不会改变或者影响代码的任何意义，而是被第三方称为消费器的程序通过反射的方式使用。
我们列出了Java默认的内建注解，一些称为元注解例如：@Target或者 @Retention，又有@Override，@SuppressWarnings，还有一些Java8相关的注解，比如：@Repeatable，@FunctionalInterface和类型注解。我们还展现了一两个结合使用反射的例子，并描述了一些使用注解的类库例如Spring， Junit，Hibernate。
注解是Java中一种分析元数据的强大机制，可以在不同的程序中担任不同的作用，例如校验，依赖注入，单元测试。 **


 
