---
title: javaweb ssh实例
date: 2016-03-01 11:23:46
categories:
- Java
tags:
- Java
---

本文介绍的一个简单基于ssh的javaweb实例。  

编写工具：eclipse  

#### 目录结构：

	webapp/          <-- 根目录  
	+- src/          <-- java目录   
	|  +-com/
	|  |   +- action/     
	|  |   +- dao/   
	|  |   |  +- impl/   
	|  |   +- model/  
	|  |   +- service/
	|  +- beans.xml
	|  +- hibernate.cfg.xml
	|  +- log4j.properties
	|  +- struts.xml  
	+- WebContent/   <-- Web目录   
	|  +- web-inf/  
	|  |  +- lib/    <-- jar存放位置   
	|  |  +- web.xml <-- 配置文件   
	|  +- index.jsp  <-- 主页  
	|  +- logo.jsp  

#### web.xml配置文件：

	<?xml version="1.0" encoding="UTF-8"?>
	<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://java.sun.com/xml/ns/javaee"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
	http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" id="WebApp_ID" version="3.0">
  	<display-name>webapp</display-name>
  	<welcome-file-list>
    <welcome-file>index.jsp</welcome-file>
  	</welcome-file-list>
  	<filter>
    <filter-name>struts2</filter-name>
    <filter-class>
	org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter
	</filter-class>
  	</filter>
  	<filter-mapping>
    <filter-name>struts2</filter-name>
    <url-pattern>/*</url-pattern>
  	</filter-mapping>
	</web-app>

#### jar文件：

![](http://i.imgur.com/SQZCvkL.png)

[下载链接](http://pan.baidu.com/s/1dEokkud "下载链接")

#### index.jsp代码：

	<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
	<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
	"http://www.w3.org/TR/html4/loose.dtd">
	<html>
	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
	</head>
	<body>
	<form action="logo.action">
		<input type="submit" />hello
	</form>
	</body>
	</html>

#### logo.jsp代码：

	<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
	<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
	"http://www.w3.org/TR/html4/loose.dtd">
	<html>
	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
	</head>
	<body>
	success
	</body>
	</html>

#### beans.xml配置文件：

	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">

	<bean id="u" class="com.dao.impl.UserDAOImpl">
	</bean>

	<bean id="userService" class="com.service.UserService">
		<property name="userDAO" ref="u" />
	</bean>

	</beans>

#### hibernate.cfg.xml配置文件：

	<?xml version='1.0' encoding='utf-8'?>
	<!DOCTYPE hibernate-configuration PUBLIC
        "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
        "http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd">

	<hibernate-configuration>

	<session-factory>

		<!-- Database connection settings -->
		<property name="connection.driver_class">com.mysql.jdbc.Driver</property>
		<property name="connection.url">jdbc:mysql://localhost:3306/数据库名</property>
		<property name="connection.username">mysql用户名</property>
		<property name="connection.password">mysql用户密码</property>
		
		<!-- if table null create -->
		<property name="hibernate.hbm2ddl.auto">update</property>
		
		<!-- SQL dialect -->
		<property name="dialect">org.hibernate.dialect.MySQLDialect</property>

		<!-- Disable the second-level cache -->
		<property name="cache.provider_class">
		org.hibernate.cache.NoCacheProvider</property>

		<!-- Echo all executed SQL to stdout -->
		<property name="show_sql">true</property>

		<mapping class="com.model.User" />
	</session-factory>

	</hibernate-configuration>

#### log4j.properties配置文件：

	log4j.appender.stdout=org.apache.log4j.ConsoleAppender
	log4j.appender.stdout.Target=System.out
	log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
	log4j.appender.stdout.layout.ConversionPattern=%d{ABSOLUTE} %5p %c:%L - %m%n
	log4j.rootLogger=warn, stdout
	log4j.logger.org.hibernate.tool.hbm2ddl=debug

#### struts.xml配置文件：

	<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE struts PUBLIC
    "-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
    "http://struts.apache.org/dtds/struts-2.0.dtd">

	<struts>
	<package name="key" extends="struts-default">
		<action name="logo" class="com.action.UserAction" method="add">
			<result>
				/logo.jsp
			</result>
		</action>
	</package>
	</struts>

#### com/model/User.java代码：

	import java.io.Serializable;

	import javax.persistence.Column;
	import javax.persistence.Entity;
	import javax.persistence.Id;
	import javax.persistence.Table;

	@Entity
	@Table(name = "user")
	public class User implements Serializable{
	private static final long serialVersionUID = 3891096235247216638L;
	@Id
	@Column(name = "uid")
	private String uid;
	@Column(name = "username", length = 30, nullable = false)
	private String username;
	@Column(name = "password", length = 30, nullable = false)
	private String password;
	
	public String getUid() {
		return uid;
	}
	public void setUid(String uid) {
		this.uid = uid;
	}
	public String getUsername() {
		return username;
	}
	public void setUsername(String username) {
		this.username = username;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}
	
	}

#### com/dao/UserDAO.java代码：

	import com.model.User;


	public interface UserDAO {
	public void save(User user);
	}

#### com/dao/impl/UserDAOImpl.java代码：

	import org.hibernate.Session;
	import org.hibernate.SessionFactory;
	import org.hibernate.cfg.AnnotationConfiguration;

	import com.dao.UserDAO;
	import com.model.User;


	public class UserDAOImpl implements UserDAO {

	public void save(User user) {
		AnnotationConfiguration cfg = new AnnotationConfiguration().configure();
		SessionFactory sf = cfg .buildSessionFactory();
		Session session=sf.openSession();
		session.beginTransaction();
		session.save(user);
		session.getTransaction().commit();
		session.close();
		sf.close();
		System.out.println("user saved!");
	}

	}

#### com/service/UserService.java代码：

	import com.dao.UserDAO;
	import com.model.User;

	public class UserService {
	private UserDAO userDAO;  
	public void add(User user) {
		userDAO.save(user);
	}
	public UserDAO getUserDAO() {
		return userDAO;
	}
	public void setUserDAO(UserDAO userDAO) {
		this.userDAO = userDAO;
	}
	}

#### com/action/UserAction.java代码：

	import org.springframework.context.ApplicationContext;
	import org.springframework.context.support.ClassPathXmlApplicationContext;

	import com.model.User;
	import com.opensymphony.xwork2.ActionSupport;
	import com.service.UserService;

	public class UserAction extends ActionSupport{
	public String add() {
		ApplicationContext ctx = new ClassPathXmlApplicationContext("beans.xml");
		UserService service = (UserService)ctx.getBean("userService");
		User u = new User();
		u.setUid("3");
		u.setUsername("zhangsan");
		u.setPassword("zhangsan");
		service.add(u);
		return SUCCESS;
	}
	}

以上就是简单的javaweb ssh实例。

原创文章转载请注明出处：[javaweb ssh实例](http://arcoder.com/code/2016/03/01/code-javassh.html)
