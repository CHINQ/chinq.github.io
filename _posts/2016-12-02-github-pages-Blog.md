---
title: 利用github-pages jekyll搭建个人博客
date: 2016-12-02
categories:
- jekyll
tags:
- jekyll
---
Github很好的将代码和社区联系在了一起，于是发生了很多有趣的事情，世界也因为他美好了一点点。Github作为现在最流行的代码仓库，已经得到很多大公司和项目的青睐，比如jQuery、Twitter等。为使项目更方便的被人理解，介绍页面少不了，甚至会需要完整的文档站，Github替你想到了这一点，他提供了Github Pages的服务，不仅可以方便的为项目建立介绍站点，也可以用来建立个人博客。

## 前言

前面的git等一系列的配置就不多说了。
    
在决定搭建个人博客之前，在网上看过许多攻略，并且花了很多时间在挑选主题上。  
    
为了快速的搭建博客，最后决定用jekyll-theme [NexT](https://github.com/Simpleyyt/jekyll-theme-next)主题。
    
把代码托管到git上。
    
## 搭建过程

首先创建一个特殊的代码仓库，名字必须是xxx.github.io，xxx是你的github昵称。
   
![]({{ site.url }}/assets/images/blog/blog1.jpg)
    
把NexT主题代码clone到本地再上传到刚才新建的代码仓库。
   
接下来再去申请域名，这里就不多说了，网上攻略很多。
    
再去从刚才拷贝下来的代码里找到CNAME文件，里面的内容修改成自己申请的域名地址。
   
![]({{ site.url }}/assets/images/blog/blog2.jpg)
    
这样 你就可以打开你自己的网站了。
   
接下来 你就可以 根据 [NexT主题使用文档](http://theme-next.simpleyyt.com/)搭建你自己的博客了。
    
    
## 第三方

配置文件是_config.yml
   
评论系统  [来必力](https://livere.com/)  按照流程走下来获得LiveRe UID，再在配置文件里找到livere_uid: #your livere_uid。
   
数据统计与分析  [百度统计](https://tongji.baidu.com/web/25102484/homepage/index)
          
文章阅读次数统计 [LeanCloud](https://leancloud.cn/)
    