---
title: vue-cli2.0安装说明
password: 1
date: 2019-01-07 13:26:22
tags:
categories:
---

 https://blog.csdn.net/wulala_hei/article/details/80488674

 都说Vue2简单上手容易，的确，看了官方文档确实觉得上手很快，除了ES6语法和webpack的配置让你感到陌生，重要的是思路的变换，以前用jq随便拿全局变量和修改dom的锤子不能用了，vue只用关心数据本身，不用再频繁繁琐的操作dom，注册事件、监听事件、取消事件。。。。（确实很烦）。vue的官方文档还是不错的，由浅到深，如果不使用构建工具确实用的很爽，但是这在实际项目应用中是不可能的，当用vue-cli构建一个工程的时候，发现官方文档还是不够用，需要熟练掌握es6，而vue的全家桶(vue-cli,vue-router,vue-resource,vuex)还是都要上的。

vue.js有著名的全家桶系列，包含了vue-router，vuex， vue-resource，再加上构建工具vue-cli，就是一个完整的vue项目的核心构成。

vue-cli这个构建工具大大降低了webpack的使用难度，支持热更新，有webpack-dev-server的支持，相当于启动了一个请求服务器，给你搭建了一个测试环境，只关注开发就OK。

1.安装vue-cli
① 使用npm（需要安装node环境）全局安装webpack，打开命令行工具输入：npm install webpack -g或者（npm install -g webpack），安装完成之后输入 webpack -v，如下图，如果出现相应的版本号，则说明安装成功。

注意：webpack 4.X 开始，需要安装 webpack-cli 依赖 ,所以使用这条命令  

##npm install webpack webpack-cli -g

② 全局安装vue-cli，在cmd中输入命令:

##npm install --global vue-cli

安装成功：

安装完成之后输入 vue -V（注意这里是大写的“V”），如下图，如果出现相应的版本号，则说明安装成功。

##vue -V
输出 2.9.3 

打开C:\Users\Andminster\AppData\Roaming\npm目录下可以看到：

打开node_modules也可以看到：

2.用vue-cli来构建项目
① 我首先在D盘新建一个文件夹（dxl_vue）作为项目存放地，然后使用命令行cd进入到项目目录输入：

##vue init webpack baoge

baoge是自定义的项目名称，命令执行之后，会在当前目录生成一个以该名称命名的项目文件夹。

输入命令后，会跳出几个选项让你回答：

Project name (baoge)： -----项目名称，直接回车，按照括号中默认名字（注意这里的名字不能有大写字母，如果有会报错Sorry, name can no longer contain capital letters），阮一峰老师博客为什么文件名要小写 ，可以参考一下。

Project description (A Vue.js project)： ----项目描述，也可直接点击回车，使用默认名字

Author ()： ----作者，输入你的大名

接下来会让用户选择：
Runtime + Compiler: recommended for most users 运行加编译，既然已经说了推荐，就选它了

Runtime-only: about 6KB lighter min+gzip, but templates (or any Vue-specificHTML) are ONLY allowed in .vue files - render functions are required elsewhere 仅运行时，已经有推荐了就选择第一个了

Install vue-router? (Y/n) 是否安装vue-router，这是官方的路由，大多数情况下都使用，这里就输入“y”后回车即可。

Use ESLint to lint your code? (Y/n) 是否使用ESLint管理代码，ESLint是个代码风格管理工具，是用来统一代码风格的，一般项目中都会使用。

接下来也是选择题Pick an ESLint preset (Use arrow keys) 选择一个ESLint预设，编写vue项目时的代码风格，直接y回车

Setup unit tests with Karma + Mocha? (Y/n) 是否安装单元测试，我选择安装y回车

Setup e2e tests with Nightwatch(Y/n)? 是否安装e2e测试 ，我选择安装y回车

回答完毕后上图就开始构建项目了。

② 配置完成后，可以看到目录下多出了一个项目文件夹baoge，然后cd进入这个文件夹：
安装依赖：

##npm install
 ( 如果安装速度太慢。可以安装淘宝镜像，打开命令行工具，输入：
 npm install -g cnpm --registry=https://registry.npm.taobao.org
 然后使用cnpm来安装 )


npm install ：安装所有的模块，如果是安装具体的哪个个模块，在install 后面输入模块的名字即可。而只输入install就会按照项目的根目录下的package.json文件中依赖的模块安装（这个文件里面是不允许有任何注释的），每个使用npm管理的项目都有这个文件，是npm操作的入口文件。因为是初始项目，还没有任何模块，所以我用npm install 安装所有的模块。安装完成后，目录中会多出来一个node_modules文件夹，这里放的就是所有依赖的模块。

然后现在，baoge文件夹里的目录是这样的：


3.启动项目

##npm run dev

如果浏览器打开之后，没有加载出页面，有可能是本地的 8080 端口被占用，需要修改一下配置文件 config里的index.js

还有，如果本地调试项目时，建议将build 里的assetsPublicPath的路径前缀修改为 ' ./ '（开始是 ' / '），因为打包之后，外部引入 js 和 css 文件时，如果路径以 ' / ' 开头，在本地是无法找到对应文件的（服务器上没问题）。所以如果需要在本地打开打包后的文件，就得修改文件路径。
我的端口没有被占用，直接成功（服务启动成功后浏览器会默认打开一个“欢迎页面”）：



注意：在进行vue页面调试时，一定要去谷歌商店下载一个vue-tool扩展程序。
4.vue-cli的webpack配置分析
从package.json可以看到开发和生产环境的入口。

可以看到dev中的设置，build/webpack.dev.conf.js，该文件是开发环境中webpack的配置入口。
在webpack.dev.conf.js中出现webpack.base.conf.js，这个文件是开发环境和生产环境，甚至测试环境，这些环境的公共webpack配置。可以说，这个文件相当重要。
还有config/index.js 、build/utils.js 、build/build.js等，具体请看这篇介绍：
https://segmentfault.com/a/1190000008644830
5.打包上线
注意，自己的项目文件都需要放到 src 文件夹下。
在项目开发完成之后，可以输入 npm run build 来进行打包工作。

##npm run build
另：

1.npm 开启了npm run dev以后怎么退出或关闭？
ctrl+c
2.--save-dev
自动把模块和版本号添加到模块配置文件package.json中的依赖里devdependencies部分
3. --save-dev 与 --save 的区别
--save     安装包信息将加入到dependencies（生产阶段的依赖）
--save-dev 安装包信息将加入到devDependencies（开发阶段的依赖），所以开发阶段一般使用它
打包完成后，会生成 dist 文件夹，如果已经修改了文件路径，可以直接打开本地文件查看。
项目上线时，只需要将 dist 文件夹放到服务器就行了。


##vue项目说明

1.通过  main.js  

主要引入  router    axios     store   filters

全局注册过滤器

Object.keys(filters).forEach(key => {
	Vue.filter(key,filters[key]);
});  

2.  入口文件有了，接下来该看看 router了  路由文件

 正常引入 vue 和  router

路由地址 import err401 from "../views/error/err401.vue"

上面属于静态组件路由地址

注册路由  Vue.use(Router);

constRouterMap = [{
	path:"",
	name:"",
	component:"",
	hidden:false,
},{},];

export default new Router({
	routers: constRouterMap,
	strict: process.env.Node_ENV !== "false"
})

export const asyncRouterMap = [
	{
		path:"",
		redirect:"/usereeererff/index",
		component:Home,
		meta: {
			authRule:["user_manage/admin_manage"]
		},
		children:[
			{
				path:

			},
		]
	}
];

子路由 ：  www.baidu.com/user    子:  www.baidu.com/user/authAdmin


3.接下来，，我们要看的是 views层，，相关联的为components层

views 页面层  ： 按照ps的切图，划分组件


4.assets  存放静态资源文件


5.对于开发环境 和  测试环境  得需要一个开关进行相关的切换。 那么看一下 config文件夹

const  base_url = process.env.VUE_APP_API_BASE;


6. 既然有开发，测试的切换就有， 那就看看api吧

api名字   view 层相对应上  eg:  user.js  --->   export function xxx (){};  export function xxx2 (){};


7. 针对上面提出的   错误处理  捕获 所有异常 axios 怎么搞 ？


8. 在写上面数据的时候， 避免不了的要提取出来一些methods，看看 utils  folder

可以放  api  捕获异常  错误信息的   全局共有方法  按照功能区分开来

9. 对于数据处理  filters 


10. directive 指令 自定义指令 像： @click  ,   v-bind  ,  v-modal  等不够用，可以自定义指令

自定义指令是用来操作DOM的。尽管Vue推崇数据驱动视图的理念，但并非所有情况都适合数据驱动。自定义指令就是一种有效的补充和扩展，不仅可用于定义任何的DOM操作，并且是可复用的。

directives 里面没什么可说的，不过很多难题都可以通过他来解决，要时刻记住，我们可以再指令里面操作虚拟 DOM。

但凡遇到第三方插件如何与Vue.js集成的问题，都可以尝试用自定义指令实现。

它有几个钩子函数: bind  -->   inserted  -->   update  --> componentUpdated  --> unbind

<!DOCTYPE html>
<html>

    <head>
        <meta charset="UTF-8">
        <title></title>
        <script src="vue2.2.js"></script>
    </head>

    <body>
        <div id="app">
            <div v-lang="color">{{num}}</div>
            <p><button @click="add">add</button></p>
        </div>
        <p>
            <button onclick='unbind()'>解绑</button>
        </p>
    </body>
    <script type="text/javascript">
        function unbind() {
            vm.$destroy();//另外起一个方法解绑
        }
        Vue.directive('lang', { //五个注册指令的钩子函数
            bind: function() { //被绑定
                console.log('1 - bind');
            },
            inserted: function() { //绑定到节点
                console.log('2 - inserted');
            },
            update: function() { //组件更新
                console.log('3 - update');
            },
            componentUpdated: function() { //组件更新完成
                console.log('4 - componentUpdated');
            },
            unbind: function() { //解绑
                console.log('5 - bind');
            }
        })
        var vm = new Vue({
            el: "#app",
            data: {
                num: 10,
                color: 'red'
            },
            methods: {
                add: function() {
                    this.num++;
                }
            }
        })
    </script>

</html>

1、bind:只调用一次，指令第一次绑定到元素时调用，用这个钩子函数可以定义一个绑定时执行一次的初始化动作。

2、inserted:被绑定元素插入父节点时调用（父节点存在即可调用，不必存在于document中）。

3、update:被绑定于元素所在的模板更新时调用，而无论绑定值是否变化。通过比较更新前后的绑定值，可以忽略不必要的模板更新。

4、componentUpdated:被绑定元素所在模板完成一次更新周期时调用。

5、unbind:只调用一次，指令与元素解绑时调用。






