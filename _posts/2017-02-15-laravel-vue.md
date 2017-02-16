---
layout: post
title: '[VueCms系列] 项目初始化——Laravel 5.4 + Vuejs 2.0'
date: '2017-02-15 16:24:10 +0800'
tags: 'VueCms, Vuejs'
categories: 'VueCms'
---
# 1.安装Laravel 5.4
## 1.1. 前期配置
Laravel使用Composer管理依赖，使用NPM管理前端资源。因此，使用Laravel之前，需要确保这些软件已被安装。

* Composer：[https://getcomposer.org/download/](https://getcomposer.org/download/)
* Nodejs：[https://nodejs.org/en/](https://nodejs.org/en/) (Window环境下，NPM会随新版Nodejs一并安装，不需要单独安装)
* Yarn：[https://yarnpkg.com/en/docs/install](https://yarnpkg.com/en/docs/install) (新的包管理工具，可取代NPM)

下载完成后，安装即可。因国内网络环境而导致包无法下载时，推荐使用国内镜像。

* Composer中国全量镜像：[https://pkg.phpcomposer.com/](https://pkg.phpcomposer.com/)
* 淘宝NPM镜像：[http://npm.taobao.org/](http://npm.taobao.org/)

## 1.2. 下载项目文件
[Laravel官方文档](https://laravel.com/docs/5.4)推荐两种安装方式。

* 通过 Laravel 安装器
* 通过 Composer Create-Project

我一般使用第二种方法安装Laravel。但在第二种方法中，Composer会根据当前环境下的PHP版本下载对应的Laravel项目，我在windows环境中安装了PHP 5.5，因此得到的是Laravel 5.2。
这里介绍一个更加通用的安装方法：通过git clone克隆项目文件。

    git clone https://github.com/laravel/laravel.git

通过这种方式下载的是原始项目，没有安装依赖，需要自行安装。在项目根目录下执行composer命令，安装依赖。

    composer install

执行完后，还需要安装NPM依赖。如果讨厌NPM一些奇怪的设计方式，这里推荐Yarn包管理工具，可以实现同样的功能，并且更加快速方便。

    set SASS_BINARY_SITE=https://npm.taobao.org/mirrors/node-sass/      //解决node-sass安装失败的问题
    yarn install                                                        //npm安装方式：npm install 

PS：在git CMD中运行npm会出现很多奇怪的问题，建议使用系统自带的CMD工具。

## 1.3. 配置Laravel
首先，配置运行环境。复制项目根目录下的.env.example文件，并重命名为.env（windows环境不支持该命名方式，可通过sublime等编辑器进行重命名）。
当前，我们需要配置数据库信息。
    DB_DATABASE=vuecms    //数据库名
    DB_USERNAME=kapeter   //用户名
    DB_PASSWORD=123456    //密码