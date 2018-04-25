---
title: centos搭建git服务端
date: 2016/4/6 8:50:19 
categories:
- Code
tags:
- Code
---
最近买了个VPS搭建SS,想着太浪费了,就准备搭建个git服务器，存放一些私人文件。  
vps的环境是Centos。本机的环境是Windows10，安装的git客户端。

#### 1. 安装GIT

	# yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel perl-devel  
    # yum install git  

创建一个git用户组和用户，用来运行git服务：  

	# groupadd git  
	# adduser git -g git   

#### 2. 安装SSH

	# yum install ssh

启动SSH

	# service sshd start

设置开机运行

	# chkconfig sshd on

#### 3. 创建证书

证书这一步搞来搞去，一直认证不成功，囧。当然你也可以不建证书，每次都输入密码，那这步就可略过了。  
在本地利用ssh 生成id\_rsa.pub(公钥)，id\_rsa(私钥)  
启动一个Git Bash窗口

	ssh-keygen -t rsa -C "your_email@youremail.com"  

然后会提示输入密码，这个弄不弄都行。  
最后会在C:\Users\用户\\.ssh下生成id_rsa和id_rsa.pub文件  
把公钥id\_rsa.pub发送到服务器上（我是先用密码clone了项目，然后把密钥传上去的，虽说可以粘贴，但是我也不知道我开始认证不成功有没有这个因素,我最开始就是粘贴的。。。）  
找到git用户目录  

	# cd /home/git/  

找到.ssh/authorized_keys 如果没有，可以自行创建    

把公钥添加到authorized_keys中  

	# sudo cat id_rsa.pub>>authorized_keys  

虽说有自动添加的命令，将公钥传送到远程主机host上面，我试了试，但没有成功。。。  

	# ssh-copy-id user@host  

修改.ssh和authorized_keys的权限

	# sudo chmod 700 .ssh  
	# sudo chmod 600 authorized_keys  

配置sshd_config   

	#vi /etc/ssh/sshd_config  

	RSAAuthentication yes  
	PubkeyAuthentication yes  
	AuthorizedKeysFile .ssh/authorized_keys
	AuthorizedKeysFile	 %h/.ssh/authorized_keys   # 验证的rsa key的文件在对应的用户目录下面

#### 4. 创建一个git仓库

	# cd /home
	# mkdir gitspace
	# chown git:git gitspace/
	# cd gitrepo

	# git init --bare resources.git
	Initialized empty Git repository in /home/gitspace/resources.git/

以上命令Git创建一个空仓库，服务器上的Git仓库通常都以.git结尾。然后，把仓库所属用户改为git：  

	# chown -R git:git resources.git  

#### 5. 修改端口

往往因为默认端口被占用或者权限不够等等原因，一直提示默认端口22拒绝链接，我们不得不重新定义自己的端口。  
修改ssh的配置信息 ~/.ssh/config

	#git server  
	Host username  
	HostName ip  
	User name  
	Port port  
	IdentityFile ~/.ssh/authorized_keys 


#### 6. 重新启动服务

	# /etc/init.d/sshd start

#### 7. 克隆仓库

	# git clone ssh://username@xxx.xxx.xxx.xxx:xxxx/home/gitspace/resources.git  

	Cloning into 'resources'...
	warning: You appear to have cloned an empty repository.
	Checking connectivity... done.  

username:服务器账号名字，我用的root...  
@后面接的是服务器地址和端口号（与VPS给的端口号一致！！！最开始，我设置映射端口，但是没有效果，仍提示拒绝链接，这个可能是与我vps有关，我用的是Bandwagon，后来发现只能用他提供的端口，其他vps可能没有此问题，好坑）  
  
这种方法是默认的端口所以我就用了上面的那种。  

	# git clone git@xxx.xxx.xxx.xxx:/home/gitspace/resources.git






