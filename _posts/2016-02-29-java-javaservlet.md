---
title: javaweb servlet实例
date: 2016-02-29 11:23:46
categories:
- Java
tags:
- Java
---

本文介绍的是一个简单的javaweb实例。通过jsp、xml、servlet实现数据和视图的传递。  

编写工具：eclipse  
目录结构：

	webapp/          <-- 根目录  
	+- src/          <-- java目录   
	|  +- bo/     
	|  +- dao/   
	|  |  +- impl/   
	|  +- filter/  
	|  +- servlet/  
	+- WebContent/   <-- Web目录   
	|  +- web-inf/  
	|  |  +- lib/    <-- jar存放位置   
	|  |  +- web.xml <-- 配置文件   
	|  +- web/       <-- jsp文件  
	|  +- index.jsp  <-- 主页  

xml配置 代码：  

	<?xml version="1.0" encoding="UTF-8"?>
	<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://java.sun.com/xml/ns/javaee"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
	http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
	id="WebApp_ID" version="3.0">
	<display-name>webapp</display-name>
	<welcome-file-list>
		<welcome-file>index.jsp</welcome-file>
	</welcome-file-list>

	<filter>
		<display-name>CharacterFilter</display-name>
		<filter-name>CharacterFilter</filter-name>
		<filter-class>filter.CharacterFilter</filter-class>
	</filter>
	<filter-mapping>
		<filter-name>CharacterFilter</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>

	<servlet>
		<description></description>
		<display-name>LoginServlet</display-name>
		<servlet-name>LoginServlet</servlet-name>
		<servlet-class>servlet.LoginServlet</servlet-class>
	</servlet>
	<servlet-mapping>
		<servlet-name>LoginServlet</servlet-name>
		<url-pattern>/login</url-pattern>
	</servlet-mapping>

	</web-app>

lib中存放的jar文件：  

	mysql.jar
	jstl.jar

index.jsp 代码：  

	<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
	<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
	<!DOCTYPE>
	<html>
	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
	</head>
	<body>
	<form action="login" method="post">
		<table align="center">
			<tr>
				<td>用户名： <input type="text" name="name"></input>
				</td>
			</tr>
			<tr>
				<td>密&nbsp码： <input type="password" name="pass"></input>
				</td>
			</tr>
			<tr align="center">
				<td><input type="submit"></input></td>
			</tr>
		</table>
	</form>
	</body>
	</html>

web/main.jsp 代码：  

	<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
	<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
	<!DOCTYPE>
	<html>
	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
	</head>
	<body>
	<table>
		<tr>
			<td>name:${user.name}</td>
		</tr>
	</table>
	</body>
	</html>

filter/CharacterFilter.java 代码：  

	import java.io.IOException;
	import javax.servlet.Filter;
	import javax.servlet.FilterChain;
	import javax.servlet.FilterConfig;
	import javax.servlet.ServletException;
	import javax.servlet.ServletRequest;
	import javax.servlet.ServletResponse;

	public class CharacterFilter implements Filter {
    public CharacterFilter() {
    }

	/**
	 * @see Filter#destroy()
	 */
	public void destroy() {
		System.out.println("-----End-----");
	}

	/**
	 * @see Filter#doFilter(ServletRequest, ServletResponse, FilterChain)
	 */
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
		request.setCharacterEncoding("utf-8");
		response.setCharacterEncoding("utf-8");	
		chain.doFilter(request, response);
	}

	/**
	 * @see Filter#init(FilterConfig)
	 */
	public void init(FilterConfig fConfig) throws ServletException {
		System.out.println("-----Star-----");
	}

	}

databsae/DBHelp.java 代码：  

	import java.sql.DriverManager;
	import java.sql.ResultSet;
	import java.sql.SQLException;
	import java.sql.Statement;
	import java.sql.PreparedStatement;
	import java.sql.Connection;

	public class DBHelp {
	private final String DRIVER="com.mysql.jdbc.Driver";
	private final String URL="jdbc:mysql://127.0.0.1:3306/数据库名称";
	private final String USER="mysql用户名";
	private final String PASS="mysql用户密码";
	private Connection conn;
	private PreparedStatement prep;
	private Statement stmt;

	public DBHelp() {
		try {
			Class.forName(DRIVER);
			conn=DriverManager.getConnection(URL,USER,PASS);
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}
	public int update(String sql) throws SQLException{
		stmt=conn.createStatement();
		int temp=stmt.executeUpdate(sql);
		return temp;
	}
	public int update(String sql,Object ... arr) throws SQLException{
		prep=conn.prepareStatement(sql);
		for(int i=0;i<arr.length;i++){
			prep.setObject(i+1, arr[i]);
		}
		int temp=prep.executeUpdate();
		return temp;
	}
	public ResultSet query(String sql) throws SQLException{
		stmt=conn.createStatement();
		ResultSet rs=stmt.executeQuery(sql);
		return rs;
	}
	public ResultSet query(String sql,Object ... arr) throws SQLException{
		prep=conn.prepareStatement(sql);
		for(int i=0;i<arr.length;i++){
			prep.setObject(i+1, arr[i]);
		}
		return prep.executeQuery();
	}
	public void close() throws SQLException{
		if(stmt!=null && !stmt.isClosed())stmt.close();
		if(prep!=null && !prep.isClosed())prep.close();
		if(conn!=null && !conn.isClosed())conn.close();
	}
	}

bo/User.java 代码：  

	public class User {
	private String uid;
	private String name;
	private String pass;

	public String getUid() {
		return uid;
	}
	public void setUid(String uid) {
		this.uid = uid;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getPass() {
		return pass;
	}
	public void setPass(String pass) {
		this.pass = pass;
	}
	public User(String uid,String name, String pass) {
		super();
		this.uid=uid;
		this.name = name;
		this.pass = pass;
	}
	}

servlet/LoginServlet.java 代码：

	import java.io.IOException;
	import javax.servlet.ServletException;
	import javax.servlet.http.HttpServlet;
	import javax.servlet.http.HttpServletRequest;
	import javax.servlet.http.HttpServletResponse;

	import bo.User;
	import dao.impl.LoginDaoImpl;

	public class LoginServlet extends HttpServlet{
	/**
	 * @see HttpServlet#HttpServlet()
	 */
	public LoginServlet() {
		super();
	}

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, 
	 * HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request,
	HttpServletResponse response) throws ServletException, IOException {
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response)
	throws ServletException, IOException {
	}
	@Override
	protected void service(HttpServletRequest request, HttpServletResponse response)
	throws ServletException, IOException {
		String name=request.getParameter("name");
		String pass=request.getParameter("pass");
		LoginDaoImpl impl=new LoginDaoImpl();
		User u=impl.view(name,pass);
		response.setContentType("text/html;charset=UTF-8"); 
		if(u==null){
			response.sendRedirect("index.jsp");
		}else{
			request.setAttribute("user",u);
			request.getRequestDispatcher("web/main.jsp").forward(request,response);
		}
	}
	}

dao/impl/LoginDaoImpl.java 代码：  

	import bo.User;
	import dao.LoginDao;

	public class LoginDaoImpl {
	private LoginDao logindao;
	public LoginDaoImpl(){
		logindao=new LoginDao();
	}
	public User view(String name,String pass){
		return logindao.view(name,pass);
	}
	}

dao/LoginDao.java 代码：  

	import java.sql.ResultSet;
	import java.sql.SQLException;

	import bo.User;
	import database.DBHelp;

	public class LoginDao {
	private DBHelp hp;
	public User view(String name,String pass){
		String sql="select * from user where username='"+name+"' and password='"+pass+"'";
		hp=new DBHelp();
		User user=null;
		try {
			ResultSet rs=hp.query(sql);
			if(rs.next()){
				user=new User(rs.getString(1),rs.getString(2),rs.getString(3));
			}
		} catch (SQLException e) {
			e.printStackTrace();
		}finally{
			try {
				hp.close();
			} catch (SQLException e) {
				e.printStackTrace();
			}
		}
		return user;
	}
	}

数据库模型：  

	表名：user  

<table>
<thead>
<tr>
  <th>列名</th>
  <th>数据类型</th>
  <th>长度</th>
</tr>
</thead>
<tbody>
<tr>
  <td>uid</td>
  <td>varchar</td>
  <td>32</td>
</tr>
<tr>
  <td>username</td>
  <td>varchar</td>
  <td>32</td>
</tr>
<tr>
  <td>password</td>
  <td>varchar</td>
  <td>32</td>
</tr>
</tbody>
</table>

以上就是javaweb的一个简单实例。

原创文章转载请注明出处：[javaweb servlet实例](http://arcoder.com/code/2016/02/29/code-javaservlet.html)