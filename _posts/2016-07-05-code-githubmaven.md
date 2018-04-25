---
layout: post
title: GitHub构建个人Maven仓库
date: 2016/7/5 9:45:34 
category: "code"
---

Maven是一个项目管理工具，依赖管理功能极其便利。我们可以利用GitHub来发布jar包，并利用它的raw服务提供对外下载功能。

#### 准备

配置好ssh-key的GitHub账户 [https://github.com ](https://github.com )   
git运行环境 [http://git-scm.com](http://git-scm.com)   
maven运行环境 [http://maven.apache.org](http://maven.apache.org)   

#### 搭建

创建一个新的GitHub仓库，Copy地址：git@github.com:CHINQ/maven.git  
打开maven本地仓库.m2/repository，初始化git本地仓库，添加远程地址：  

	cd ~/.m2/repository
	git init
	git remote add origin git@github.com:CHINQ/maven.git

创建.gitignore文件并提交：  

	echo *>>.gitignore
	git add .gitignore
	git commit -m 'add .gitignore'

创建分支并提交：  

	git branch snapshot
	git push origin snapshot
	git checkout snapshot

找到你要发布的.jar文件，将它部署到本地Maven仓库：  

	mvn install:install-file -Dfile=spring-core-3.2.10.RELEASE.jar -DgroupId=com.github.CHINQ.spring -DartifactId=spring-core -Dversion=3.2.10 -Dpackaging=jar

将本地Maven仓库对应的文件提交到GitHub：  

	cd ~/.m2/repository
	git add -f com/github/CHINQ/spring/spring-core/
	git commit -m 'snapshot of spring-core-3.2.10'
	git push origin snapshot

#### 使用

pom.xml：  

	<project>
	<repositories>
    	<repository>
      		<id>CHINQ-maven-snapshot-repository</id>
      		<name>CHINQ-maven-snapshot-repository</name>
      		<url>https://raw.github.com/CHINQ/maven/snapshot/</url>
    	</repository>
	</repositories> 
	<dependencies>
    	<dependency>
      		<artifactId>spring-core</artifactId>
      		<groupId>com.github.CHINQ.spring</groupId>
      		<version>3.2.10</version>
    	</dependency>
	</dependencies>
	</project>