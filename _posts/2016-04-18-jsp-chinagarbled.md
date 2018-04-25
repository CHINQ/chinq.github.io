---
title: jsp中文乱码
date: 2016/4/18 16:13:23 
categories:
- JSP
tags:
- JSP
---
今天在jsp接收参数的时候，中文变成了??，然后试了网上各种GB2312、ISO-8859-1、UTF-8转换，都不行。最后发现，在中文Windows上，默认是GBK格式，在英文Linux上，默认是ISO-8859-1格式。 

然后  

	new String(obj.getBytes("GBK"),"UTF-8");  

就Ok了。