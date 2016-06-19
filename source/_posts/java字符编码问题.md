title: java字符编码问题
date: 2015-10-20 21:19:50
tags: java
---
# 下面带来java WEB开发中字符编码所遇到的问题
最常使用解决乱码问题的就是使用过滤器
<!-- more -->


```java
import java.io.IOException;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;


public class CharsetFilter implements Filter {
	
	private FilterConfig config;
	private String charset="utf-8";
	@Override
	public void destroy() {
	}
	@Override
	public void doFilter(ServletRequest req, ServletResponse res,
			FilterChain arg2) throws IOException, ServletException {
		HttpServletRequest  request =(HttpServletRequest)req;
		HttpServletResponse response = (HttpServletResponse) res;
		charset = config.getInitParameter("charset");
		if(charset!=null && !charset.equals("")){
			request.setCharacterEncoding(charset);
			response.setCharacterEncoding(charset);  
		}
		arg2.doFilter(req, res);
	}
	@Override
	public void init(FilterConfig config) throws ServletException {
			this.config =config;
	}
}
```
web.xml中过滤器的配置
```xml
 <filter>
    <filter-name>CharsetFilter</filter-name>
    <filter-class>com.dzwz.shop.filter.CharsetFilter</filter-class>
    <init-param>
      <param-name>charset</param-name>
      <param-value>UTF-8</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>CharsetFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping> 
```
request.setCharacterEncoding(charset); 必须写在第一次使用request.getParameter()之前，这样才能保证参数是按照已经设置的字符编码来获取。
response.setCharacterEncoding(charset);必须写在PrintWriter out = request.getWriter()之前，这样才能保证out按照已经设置的字符编码来进行字符输出。
通过过滤器，我们可以保证在Servlet或JSP执行之前就设置好了请求和响应的字符编码。
但是这样并不能完全解决中文乱码问题：
对于post请求，无论是“获取参数环节”还是“输出环节"都是没问题的；
对于get请求，"输出环节"没有问题，但是"获取参数环节"依然出现中文乱码，所以在输出时直接将乱码输出了。
原因是post请求和get请求存放参数位置是不同的：
post方式参数存放在请求数据包的消息体中。get方式参数存放在请求数据包的请求行的URI字段中，以？开始以param=value&parame2=value2的形式附加在URI字段之后。而request.setCharacterEncoding(charset); 只对消息体中的数据起作用，对于URI字段中的参数不起作用，我们通常通过下面的代码来完成编码转换：
``` java 
String paramValue = request.getParameter("paramName");  
paramValue = new String(paramValue.trim().getBytes("ISO-8859-1"), charset);  
```
但是每次进行这样的转换实在是很麻烦，有没有统一的解决方案呢？

解决方案:在tomcat_home\conf\server.xml 中的Connector元素中设置URIEncoding属性为合适的字符编码
```xml
<Connector port="8080" protocol="HTTP/1.1"   
           connectionTimeout="20000"   
           redirectPort="8443"   
           URIEncoding="UTF-8"  
 />  
```
还有一种就是通过转码后在进入数据库中后会出现乱码，这时你会去查看你的数据库的编码格式在数据库的编码也是正确的时候，你会发现还是会乱码这时你会开时怀疑人生。下面以MySql举例
原因：
是在填写jdbcUrl时你没有添加useUnicode=true&characterEncoding=UTF-8
添加的作用是：指定字符的编码、解码格式。
正确的URL格式为：
```
jdbc:mysql://localhost:3306/数据库名?useUnicode=true&characterEncoding=UTF-8
```

**注意：在xml配置文件中配置数据库utl时，要使用&的转义字符也就是&amp;**

例如:
```xml
<property name="url" value="jdbc:mysql://localhost:3306/email&amp;useUnicode=true&amp;characterEncoding=UTF-8" />
```
