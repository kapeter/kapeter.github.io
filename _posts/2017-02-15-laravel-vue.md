---
layout: post
title: '[VueCms系列] 项目初始化——Laravel 5.4 + Vue.js 2.0'
date: '2017-02-15 16:24:10 +0800'
tags: 'VueCms, Vuejs, Laravel'
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
当前，我们只需要配置数据库信息。

    DB_DATABASE=vuecms    //数据库名
    DB_USERNAME=kapeter   //虚拟机中数据库的用户名
    DB_PASSWORD=123456    //虚拟机中数据库的密码

接着，登录虚拟机，创建数据库，也可以通过第三方工具可视化创建。
数据库创建完毕后，进入项目根目录，输入下列命令，进行数据库迁移。

    php artisan migrate

如果出现“Specified key was too long”的错误提示，可在app/Providers/AppServiceProvider.php添加以下代码，解决问题。

    use Illuminate\Support\Facades\Schema;

    /**
    * Bootstrap any application services.
    *
    * @return void
    */
    public function boot()
    {
        Schema::defaultStringLength(191);
    }

如果出现“Table XXX already exists”的错误提示，清空数据库中的表，再执行一遍命令，即可解决问题。
执行完后，查看数据库，如果出现表了，则说明数据迁移成功。

数据迁移完后，我们执行以下命令生成application key。

    php artisan key:generate

最后，在根目录执行以下命令打包并发布前端资源。

    npm run dev

全部配置好后，在浏览器输入网址http://homestead.app（Homestead.yaml中配置的网址），如果出现以下画面，则说明安装成功。
<img src="/images/laravel-install-success.png" alt="laravel install succeeded" width="640">

#2. 在Laravel中配置Vue.js
## 2.1. 插件配置
从5.3版本开始，Laravel使用Vue.js作为默认JavaScript前端框架，因此在前面下载NPM依赖时，Vue.js已被包含进入，不需要再次下载。但光有Vue.js，无法发挥该框架的强大功能，需要一些辅助插件来协助。

    yarn add vue-resource --dev      //Vue.js的异步请求插件，用于与后端的数据交互。
    yarn add vue-router --dev        //Vue.js的路由映射插件，两者结合可开发SPA应用。

当前，先装这两个必要的插件，后期根据项目需要，可以添加其他插件。

## 2.2. 搭建示例页面

在resources/views中，添加视图文件index.blade.php。

    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Vue.js Example</title>
        <meta name="csrf-token" content="{ { csrf_token() } }">
        <link rel="stylesheet" type="text/css" href="{ { mix('css/app.css') } }">
    </head>
    <body>
        <div id="app"></div>

        <script src="{ { mix('js/app.js') } }"></script>  
    </body>
    </html>

在routes/web.php中添加页面路由。

    Route::get('/', function () {
        return view('index');
    });

Laravel默认开启CSRF保护，我们已经在视图文件中添加了meta标签用来保存令牌，现在需要获取它，实现安全的异步请求。
打开resources/assets/js/bootstrap.js文件，找到以下代码:

    window.axios.defaults.headers.common = {
        'X-CSRF-TOKEN': window.Laravel.csrfToken,
        'X-Requested-With': 'XMLHttpRequest'
    };

替换为：

    window.axios.defaults.headers.common = {
        'X-CSRF-TOKEN': document.querySelector('meta[name="csrf-token"]').getAttribute('content'),
        'X-Requested-With': 'XMLHttpRequest'
    };

然后在resources/assets/js中，创建主视图文件App.vue。

    <template>
        <router-view></router-view>
    </template>

打开配置文件app.js，进行配置。

    import Vue from 'vue'                                         //引入vue
    import VueRouter from 'vue-router'                            //引入vue-router
    import App from './App.vue'                                   //载入主视图文件
         

    Vue.use(VueRouter)                                            //使用vue-router

    import Example from './components/Example.vue'                //引入官方提供的示例组件

    const router = new VueRouter({                                //设置路由映射
        mode: 'history',
        base: __dirname,
        routes: [
            { path: '/', component: Example }                     //在该页面中，使用Example组件
        ]
    })

    new Vue(Vue.util.extend({ router }, App)).$mount('#app');     //启动应用

由于更改了js文件，因此我们需要重新发布一遍前端资源。如果觉得手动发布比较麻烦，可以通过以下命令，实现文件监控，自动发布。

    npm run watch

现在再浏览首页，画面内容已经发生了改变，说明Vue.js配置成功。
<img src="/images/Vuejs-setting-succeeded.png" alt="Vuejs setting succeeded" width="640">
在此基础上，我们可以开发优雅的SPA应用啦。




