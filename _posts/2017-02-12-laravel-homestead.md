---
layout: post
title: '[VueCms系列] windows下搭建开发环境——Laravel Homestead'
date: '2017-02-12 19:24:10 +0800'
tags: 'VueCms, Homestead'
categories: 'VueCms'
---
# 前期配置
Homestead本质是一个vagrant盒子，因此在安装之前需要在本地配置vagrant环境。

* virtualbox：[https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)
* vagrant：[https://www.vagrantup.com/downloads.html](https://www.vagrantup.com/downloads.html)

下载完成后，安装即可。

# 安装Laravel Homestead
## 下载Homestead Vagrant盒子
在终端（我使用的是Git CMD）输入下列命令，将Homestead盒子添加到vagrant中。下载过程将会花费一些时间，时间长短取决于你的网络速度（国内用户建议开启代理）。

    vagrant box add laravel/homestead

## 通过GitHub安装Homestead