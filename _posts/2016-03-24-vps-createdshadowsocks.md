---
layout: post
title: vps搭建shadowsocks
date: 2016-03-24 13:06:26
category: "vps"
---

Shadowsocks是一个安全的Socks代理，用于保护网络流量不被干扰，也是开源的项目。主要特性包括：

快速（异步I/O和事件驱动程序）。

安全（所有的流量都经过加密算法加密，支持自定义算法）。

支持移动客户端（专为移动设备和无线网络优化）。

跨平台（可运行于包括PC，Mac，手机（Android和iOS）和路由器（OpenWrt）在内的多种平台上）。

使用Socks5协议和可自定义密码的工业级算法加密，流量在网络传输过程中不易被他人读取。

开源。

易于维护。

本文介绍的是在国外VPS环境下搭建Shadowsocks服务器的方法，服务端Shadowsocks(下文中以SS代替)是Python版本，OS是Cent OS 6。完成服务端配置后，运行本地端就可以无障碍的访问被墙的站点。其原理如下图。  

![](http://i.imgur.com/yM6K77l.png)  

## 如何搭建搬瓦工的VPS。  

#### 1.登录BandwagonHost官方网站

官网地址：[https://bandwagonhost.com/](https://bandwagonhost.com/)

![](http://i.imgur.com/YwlvtZt.png)  

登录官方网站首页，在右上角看到REGISTER按钮连接，准备注册。  

#### 2.填写个人信息  

![](http://i.imgur.com/0uC26wo.png)  

根据上图所示，我们可以注册个人信息，需要注意以下几点：  

A - 用拼音填写  

B - 资料是否完全真实无所谓，邮箱必须自己的，以免以后找回密码需要  

C - 国家、地区、省份必须真实，地址你随意填写  

很多人都是因为这里的问题导致无法付款，注意不能用V=P=N软件更换IP购买。  

#### 3.选择对应的年付优惠  

选择购买的方案，这个看个人需求。

![](http://i.imgur.com/MpUHhkg.png)

选择购买的期限和VPS的地区。

![](http://i.imgur.com/9DYao1E.png)  

输入优惠码，然后checkout。

    搬瓦工优惠码：当前我们可以使用 IAMSMART5GRNII 优惠码再节省5%

![](http://i.imgur.com/StWylVc.png)  

可以选择信用卡或支付宝付款，极为方便。

![](http://i.imgur.com/itLgBhK.png)

#### 4.付款成功以及使用

![](http://i.imgur.com/kbWBjLy.png)

## 如何搭建Shadowsocks服务器。  

登陆搬瓦工的Client Area，Services—My Services，找到你买的VPS服务，进入KiwiVM控制面板，右侧显示的就是你的VPS的信息，从上至下依次是物理地址，IP地址，SSH端口号，运行状态，操作(开关机，重启)，RAM占用，SWAP占用，磁盘占用，流量使用情况(每个月都会刷新)，和Linux操作系统版本。  

![](http://i.imgur.com/zCYH2dH.png)  

注意！！！不同操作系统下搭建SS服务器方法不同，本文的方法是基于Cent OS 6系统。如果你安装的其他Linux系统(如Debian,Ubuntu等)，请自行百度SS安装教程，或点击KiwiVM控制面板左侧的 Install New OS重装Cent OS系统。

#### 1.使用Putty连接VPS。

Putty是Windows下的一个免费的SSH连接工具，如下图示  

![](http://i.imgur.com/Gi9a5xr.png)  

Host Name处填入VPS的IP地址，Port处填入SSH端口号，连接类型选择SSH，点击Open。

用户名：root，密码：KiwiVM面板左侧Rootpassword modification，点击Generate New password后会随机生成一组密码。

#### 2.修改Root账户密码：

登陆root账户后，运行命令passwd root修改root账户密码，以后登录root账户使用该密码。

#### 3.安装SS服务端：

运行命令  

    yum update
    yum install python-setuptools && easy_install pip
    pip install shadowsocks

#### 4.配置SS服务端：

新建一个配置文件，运行命令 vi /etc/shadowsocks.json
内容如下

    {
    "server":"your_server_ip",
    "server_port":8388,
    "password":"yourpassword",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open":false,
    "workers": 1
    }

代码中各字段的含义：

server：服务器 IP地址 (IPv4/IPv6)

server_port：服务器监听的端口，一般设为80，443等，注意不要设为使用中的端口

password：设置密码，自定义

timeout：超时时间（秒）

method：加密方法，可选择 “aes-256-cfb”, “rc4-md5”等等。推荐使用 “rc4-md5”

fast_open：true 或 false。如果你的服务器 Linux 内核在3.7+，可以开启 fast_open 以降低延迟。

workers：workers数量，默认为 1。

备注：如要你需要分享SS服务给朋友或家人，可以配置多个SS账户，具体可以参考下面的代码。

    {
    "server":"your_server_ip",
    "port_password":{
     "8381":"pass1",
     "8382":"pass2",
     "8383":"pass3",
     "8384":"pass4"
     },
    "timeout":60,
    "method":"rc4-md5",
    "fast_open":false,
    "workers":1
    }

#### 5.设置Shadowsocks开机自启。

修改rc.local文件，运行命令 vi /etc/rc.local

内容如下

    #!/bin/sh
    ssserver -c /etc/shadowsocks.json -d start

运行命令 reboot 重启VPS。

#### 6.客户端配置

下载地址：[https://shadowsocks.org/en/download/clients.html](https://shadowsocks.org/en/download/clients.html)

以Windows客户端为例，进入服务器选项-编辑服务器-添加，各项参数配置如下图示。

其中各项参数按照服务端的配置填入后确定，点击右键-启用系统代理。

其他客户端的配置方法与Windows客户端基本相同。

![](http://i.imgur.com/0Y6EMyM.png)

上述所有步骤完成之后，打开浏览器即可进入被墙的站点了，例如油管，谷歌等。  

![](http://i.imgur.com/abmjjZz.png)

参考  
[https://php-rmcr7.rhcloud.com/shadowsocks-server/](https://php-rmcr7.rhcloud.com/shadowsocks-server/ "教你如何搭建自己的Shadowsocks服务器。")  