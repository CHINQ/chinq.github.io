---
title: java forEach特性
date: 2016-04-01 13:19:46
categories:
- Java
tags:
- Java
---

Journey of for loop in Java, for(index) to forEach().

    public class JavaForEach {
    	public static void main(String[]args){
    		List<String>listNames= Arrays.asList("Tom","Jerry");
    		//java 1.4
    		for (int i=0;i<listNames.size();i++)System.out.println(listNames.get(i));
   			//java 1.5
    		for (String name:listNames)System.out.println(name);
    		//java 1.8
    		listNames.stream().forEach(System.out::println);
    	}
    }