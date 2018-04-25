---
layout: post
title: javaweb springmvc实例
date: 2016-03-02 11:23:46
category: "java"
---

这是一个 Java Web 的 SpringMvc 实例。  

编写工具：eclipse

#### 目录结构：

	webapp/               			<-- 根目录  
	+- src/               			<-- java目录   
	|  +-log4j.properties
	|  +- com/
	|  |  +- sys/     
	|  |  +- common/   
	|  |  |  +- controller/     
	+- WebContent/        			<-- Web目录   
	|  +- web-inf/  
	|  |  +- lib/         			<-- jar存放位置   
	|  |  +- jsp/ 
	|  |  |  +- login.jsp
	|  |  +- web.xml 
	|  |  +- spring-mvc.xml 
	|  |  +- spring-application.xml
	|  +- index.jsp       			<-- 主页  

#### web.xml配置文件：

	<?xml version="1.0" encoding="UTF-8"?>
	<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns="http://java.sun.com/xml/ns/javaee"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
	http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" id="WebApp_ID" version="2.5">
  	<display-name>webapp</display-name>
  	<welcome-file-list>
    <welcome-file>index.jsp</welcome-file>
  	</welcome-file-list>
   	<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>
		/WEB-INF/spring-application.xml,
		/WEB-INF/spring-mvc.xml
		</param-value>
  	</context-param>
  	<listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener
	</listener-class>
  	</listener>
  	<servlet>
    <servlet-name>springmvc</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>/WEB-INF/spring-mvc.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  	</servlet>
  	<servlet-mapping>
    <servlet-name>springmvc</servlet-name>
    <url-pattern>*.do</url-pattern>
  	</servlet-mapping>
  	<listener>
    <listener-class>org.springframework.web.util.IntrospectorCleanupListener	
	</listener-class>
  	</listener>
  	<listener>
    <listener-class>org.springframework.security.web.session.HttpSessionEventPublisher
	</listener-class>
  	</listener>
  	<session-config>
    <session-timeout>240</session-timeout>
  	</session-config>
  	<filter>
    <filter-name>encodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>UTF-8</param-value>
    </init-param>
    <init-param>
      <param-name>forceEncoding</param-name>
      <param-value>true</param-value>
    </init-param>
  	</filter>
  	<filter-mapping>
    <filter-name>encodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  	</filter-mapping>
	</web-app>

#### spring-mvc.xml配置文件：

	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:security="http://www.springframework.org/schema/security"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
			http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
			http://www.springframework.org/schema/mvc
			http://www.springframework.org/schema/mvc/spring-mvc-3.0.xsd
			http://www.springframework.org/schema/context
			http://www.springframework.org/schema/context/spring-context-3.0.xsd
			http://www.springframework.org/schema/security
			http://www.springframework.org/schema/security/spring-security-3.1.xsd">
	<!-- security注解 -->
	<security:global-method-security
		pre-post-annotations="enabled" secured-annotations="enabled"
		jsr250-annotations="enabled" proxy-target-class="true" />
	<!-- 注解驱动 -->
	<mvc:annotation-driven />
	<!-- 组件扫描 context:exclude-filter是因为controller会先进行扫描装配，
		此时service还没有被事物注解加强 -->
	<context:component-scan base-package="com.cn">
		<context:exclude-filter type="annotation"
			expression="org.springframework.stereotype.Service" />
	</context:component-scan>
	<bean
		class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/jsp/" />
		<property name="suffix" value=".jsp" />
		<property name="order" value="1" />
	</bean>
	<!-- 配置自定义拦截器 -->
	<mvc:interceptors>
		<!-- 会话无缓存 -->
		<bean class="org.springframework.web.servlet.mvc.WebContentInterceptor"
			p:cacheSeconds="0" />
	</mvc:interceptors>
	</beans>

#### spring-application.xml配置文件：

	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans" 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="
	http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-2.5.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context-2.5.xsd">
	<!-- 注解扫描 -->
	<context:component-scan base-package="com" />
	<!--spring类工具 -->	
	<bean id="spring" class="com.sys.SpringUtil" /> 
	</beans>

#### jar文件

![](http://i.imgur.com/tO7fChr.png)

[下载链接](http://pan.baidu.com/s/1bZRdam "下载链接")

#### jsp/login.jsp代码：

	<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
	<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
	"http://www.w3.org/TR/html4/loose.dtd">
	<html>
	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
	</head>
	<body>
	hello world
	</body>
	</html>

#### index.jsp代码：

	<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
	<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
	"http://www.w3.org/TR/html4/loose.dtd">
	<html>
	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
	<script type="text/javascript"> 
		 	window.onload = function(){
		 		window.location="index.do";
		 	}
		</script>
	</head>
	<body>
	hello
	</body>
	</html>

#### log4j.properties配置文件：

	log4j.rootLogger=debug, stdout, R
  
	log4j.appender.stdout=org.apache.log4j.ConsoleAppender
	log4j.appender.stdout.layout=org.apache.log4j.PatternLayout

	# Pattern to output the caller's file name and line number.
	log4j.appender.stdout.layout.ConversionPattern=%5p [%t] (%F:%L) - %m%n
	log4j.appender.R=org.apache.log4j.RollingFileAppender
	log4j.appender.R.File=D:/logfile.log
	log4j.appender.R.MaxFileSize= 100KB

	# Keep one backup file
	log4j.appender.R.MaxBackupIndex=1

	log4j.appender.R.layout=org.apache.log4j.PatternLayout
	log4j.appender.R.layout.ConversionPattern=%p %t %c - %m%n 

#### com/common/controller/test.java代码：

	import org.springframework.beans.factory.annotation.Autowired;
	import org.springframework.stereotype.Controller;
	import org.springframework.web.bind.annotation.RequestMapping;
	import org.springframework.web.servlet.ModelAndView;

	@Controller   
	public class test{  
	@Autowired
    @RequestMapping("/index.do")  
    public ModelAndView index(){  
    	return new ModelAndView("/login");  
    } 
	}  
    
#### com/Sys/SpringUtil.java代码：

	import org.springframework.beans.BeansException;
	import org.springframework.context.ApplicationContext;
	import org.springframework.context.ApplicationContextAware;

	/**
 	* spring的工具类，这个类很重要，要在普通类里调用spring注入的bean对象必须通过这个类
 	* 通过注解是没用的，因为普通类不符合spring 或者 mvc机制
 	*/

	public class SpringUtil implements ApplicationContextAware {	
	private static ApplicationContext applicationContext;  
	
	@Override  
    public void setApplicationContext(ApplicationContext context)  
        throws BeansException {  
        SpringUtil.applicationContext = context;  
    }  
	
    public static Object getBean(String name){  
        return applicationContext.getBean(name);  
    }  

	}

以上就是springmvc的实例。

原创文章转载请注明出处：[javaweb springmvc实例](http://arcoder.com/code/2016/03/02/code-javaspringmvc.html)