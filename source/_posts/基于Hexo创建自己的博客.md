---
title: 基于Hexo创建自己的github.io博客
date: 2018-12-04 09:28:59
tags: Hexo
categories: 博客平台
---

## 快速搭建

如果你是有一定开发经验的前端,你的电脑一定已经安装了 git 和 node ,那你在搭建博客之前你只要先全局安装 hexo 的命令行工具,命令如下

``` bash
$ npm install -g hexo-cli
```

然后你在你的电脑里新建一个空的文件夹命名就按照 userName.github.io 的方式来命名 例如我的项目我命名为 TimeMagic.github.io, 其次,你需要进入你新建的这个文件夹,执行命令

``` bash
$ hexo init
```

等待一定时间,你会看到你新建的文件夹里出现了好多初始化文件,现在这个时候一个基本的简单博客搭建成功了,你运行以下命令

> * node_modules：是依赖包
> * public：存放的是生成的页面
> * scaffolds：命令生成文章等的模板
> * source：用命令创建的各种文章
> * themes：主题
> * _config.yml：整个博客的配置
> * db.json：source解析所得到的
> * package.json：项目所需模块项目的配置信息

``` bash
$ hexo s -p 3000
```

就能在 localhost:3000 中看到一个博客网站了,现在你要做的就是为你的博客网站做一些个性化的定制,打开_config.yml 文件.修改文件内的参数,

``` bash
title: 时间魔法师的博客              //博客网站的title
subtitle:                      	    //子标题
description: 前端,博客   			//网站描述
author: ymq             			//网站作者
language: zh-Hans            		//网站语言,用于本地化
...                          		// 等其它配置
```

## 发布博客

### 关联远程仓库

博客搭建好之后,你需要做的是把博客发布到 github 上,首先你需要在 github 上新建一个仓库,命名和你新建的文件夹一样, username.githhub.io 值得注意的是,这里的 username 必须和你的github 用户名一样,大小写也要一样.然后你需要进入你的本地文件夹,关联你的远程仓库.

首先

``` bash
$ git init
```

然后

``` bash
$ git remote add origin git@github.com:username/username.github.io.git
```

然后我们只要把代码推到远程仓库就行了,不过这里我们要有个约定,我们在 master 上发布博客,在 dev 分支上修改我们的博客内容和项目配置,也就是说我们发布博客后,master 分支上就是我们的博客的静态文件,dev 分支上的代码就是我们自己维护的博客内容

### 安装 hexo 的一键部署命令工具

通过npm安装

``` bash
$ npm install hexo-deployer-git --save
```

修改部署配置,在 _config.yml 中添加如下配置

``` bash
deploy:
  type: git
  branch: master
  repo: git@github.com:TimeMagic/TimeMagic.github.io.git  //此处记得改成你的 github 仓库的地址
```

按照约定我们先切分支,通过命令把项目切换到 dev 分支

``` bash
$ git checkout -b dev
```

然后 add 你的项目代码

``` bash
$ git add .
```

你的 first commit

``` bash
$ git commit -m ":tada: init blog"
```

然后 push 你的代码到远程

``` bash
$  git push --set-upstream origin dev
```

发布其实很简单的,我们之前也说是一个命令行发布的,所以你只要

``` bash
$ hexo deploy
```

然后你就可以看到一个属于你的 github.io 的网站了

## 改变你的博客网站主题

个人比较推荐的是 next ,大量的留白,让你的博客看起来简洁大方,或则你喜欢其它的主题也行,你需要在你的博客文件夹里clone 下你需要的主题

``` bash
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
```

然后在 _config.yml 文件夹里修改你的主题配置

``` bash
$ theme: next
```

然后 hexo s 就能看到你修改的主题生效了,为了防止 clone 下来的主题文件和自己的博客文件冲突,我删除了clone 下来的 .git 文件.你再按照你的 first commit 的操作提交你的修改就行了,然后别忘记了 deploy

``` bash
$ hexo clean
$ hexo generate
$ hexo deploy
```

## 新建一篇文章

``` bash
$ hexo new [layout] <title>
```

Hexo 有三种默认布局：post、page 和 draft，它们分别对应不同的路径，而您自定义的其他布局和 post 相同，都将储存到 source/_posts 文件夹。