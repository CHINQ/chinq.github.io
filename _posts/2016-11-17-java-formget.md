---
layout: post
title: form保存target object is not unll
date: 2016/11/17 11:37:51 
category: "java"
--- 

form提交的时候会先获取get方法，今天提交的时候发现了以前的一个Bug,创建的时候会先执行get方法，但此时数据并没有保存，代码判断此时传入的值是创建的id,返回结果根据此id查询返回null,就报了target object is not null。
 

	public Project getProject() {
		if (project != null) {
			String id = project.getId();
			if (id != null && id != "") {
				try {
					Project pro=new Project();
					pro = Project.find(id);
					if(pro!=null){
						project=pro;
					}
				} catch (AppException e) {
					LogUtil.log(this.getClass()).error(e.getMessage(),e);
				}
			}
		}
		return project;
	}

