---
layout: post
title: '[VueCms系列] windows下搭建开发环境——Laravel Homestead'
date: '2017-02-12 19:24:10 +0800'
tags: 'VueCms, Homestead'
categories: 'VueCms'
---
# 1.前期配置
Homestead本质是一个vagrant盒子，因此在安装之前需要在本地配置vagrant环境。

* virtualbox：[https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)
* vagrant：[https://www.vagrantup.com/downloads.html](https://www.vagrantup.com/downloads.html)
* Git：[https://git-scm.com/downloads](https://git-scm.com/downloads)

下载完成后，安装即可。

# 2.安装Homestead
## 2.1.下载Homestead Vagrant盒子
在终端（我使用的是Git CMD）输入下列命令，将Homestead盒子添加到vagrant中。下载过程将会花费一些时间，时间长短取决于你的网络速度（国内用户建议开启代理）。

    vagrant box add laravel/homestead

## 2.2.通过GitHub安装Homestead
通过下列命令，从github上拉取项目到本地目录。我选择把项目放在E盘的根目录中，因此我的项目路径为e:/Homestead。

    git clone https://github.com/laravel/homestead.git Homestead

下载完成后，进入项目目录，执行init.bat，创建Homestead.yaml配置文件。

# 3.配置Homestead
## 3.1.配置Homestead.yaml
可在用户目录找到Homestead.yaml文件。一般位于C:/Users/yourname/.homestead中。

    ip: "192.168.10.10"                                //虚拟机IP，供外部访问
    memory: 2048                                       //虚拟机内存数量
    cpus: 1											   //CPU核心数量
    provider: virtualbox                               //所使用的虚拟机软件
 
    authorize: ~/.ssh/id_rsa.pub                       //登录使用的公钥
 
    keys:                                              //登录使用的公钥
        - ~/.ssh/id_rsa

    folders:                                           //目录映射
        - map: E:/Project                              //本机地址，用于存放项目文件
	      to: /home/vagrant/Code                       //所对应的虚拟机目录

    sites:                                             //站点设置
        - map: homestead.app                           //所使用的域名
          to: /home/vagrant/Code/VueCms/public         //虚拟机中的网站目录

    databases:                                         //数据库
        - vuecms

了解每行代码的意义之后，可以对其进行设置。比如我需要改变文件路径，就可以设置folders的map属性。再比如我需要更改数据库，将databases中的数据库名变更即可。

## 3.2.配置Host
为了能让浏览器通过域名的方式访问网站，需要在Host中添加指向域名。Host文件位于C:/Windows/System32/drivers/etc。

    192.168.10.10   homestead.app

# 4.登录Homestead
## 4.1.配置SSH
在终端输入下列命令，配置SSH密钥对。

    ssh-keygen -t rsa -C "you@site.com"  

## 4.2.登录Homestead
进入项目目录（e:/Homestead），在终端输入下列命令启动虚拟机.

    vagrant up

启动成功后，可以通过Xshell等工具登录虚拟机。需要注意的是，登录用户名为vagrant，而不是root。

## 4.3.访问MySQL数据库
通过下列命令访问数据库。

    mysql -u homestead -p

用户名为homestead，密码为secret。使用SQLyog等远程访问同理。 
 

