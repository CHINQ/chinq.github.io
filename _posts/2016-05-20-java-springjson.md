---
layout: post
title: spring MappingJacksonJsonView
date: 2016/5/20 12:30:21 
category: "java"
---
spring 的MappingJacksonJsonView是将Model渲染成json的类。  

依赖包： jackson.jar  
在mvc-servlet.xml中添加如下的bean 定义  
  
	<bean id="beanNameViewResolver"
    	class="org.springframework.web.servlet.view.BeanNameViewResolver">
    	<property name="order" value="100" />
	</bean>
	<bean id="internalResourceViewResolver"
		class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    	<property name="prefix" value="/WEB-INF/jsp/" />
    	<property name="suffix" value=".jsp" />
	</bean>
	<bean id="userListJson" 
	class="org.springframework.web.servlet.view.json.MappingJacksonJsonView"/>

由于我们指定了BeanNameViewResolver，Spring MVC就不会默认注册InternalResourceViewResolver了所以我们需要手工注册这个ViewResolver。由于InternalResourceViewResolver会处理所有的逻辑视图到真实视图的解析，所以我们将其他ViewResolver的优先级调高（通过指定order属性，order越小，优先级越高，默认order=Integer.MAX_VALUE），让InternalResourceViewResolver成为ViewResolver链中的最后一个。  
顺便提一下BeanNameViewResolver解析的原理是，如果某个处理器（@Controller）返回的逻辑视图名和某个Bean的ID相等，那么就将Model传递给它，由它渲染。比如本例，如果返回的逻辑视图名为jsonView，那么将由MappingJacksonJsonView来生成Json。  
实现一个简单的@Controller方法  

	@RequestMapping(value="/json")
		public String json(Map<String, Object> model) {
			model.put("name", "javacoder.cn");
			model.put("list", new ArrayList<String>());
			return "userListJson";
	}


转自：[http://www.javacoder.cn/?p=180](http://www.javacoder.cn/?p=180)