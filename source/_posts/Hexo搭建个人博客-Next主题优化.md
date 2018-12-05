---
title: Hexo搭建个人博客--Next主题优化
date: 2018-12-05 09:21:33
tags: Hexo
categories: 博客平台
password: 1
---

## 站点配置文件

### 基本使用

文件路径站点根目录/_config.yml，编辑软件推荐使用Sublime Text 。

``` code
# Site
title: Alvabill
subtitle: Stay Hungry, Stay Foolish
author: Alvabill
description: "Alvabill个人站，主要涉及前端知识共享、实践教程、前沿技术共同学习等方面"  #网站描述 SEO
language: en
timezone: Asia/Shanghai
```
"title": 博客的名称。
"subtitle": 根据主题的不同，有的会显示有的不会显示。
"description": 主要用于SEO，告诉搜索引擎一个关于站点的简单描述，通常建议在其中包含网站的关键词。
"author": 作者名称，用于主题显示文章的作者。
"language": 语言会对应的解析正在应用的主题中的languages文件夹下的不同语言文件。所以这里的名称要和languages文件夹下的语言文件名称一致。
"timezone": 可不填写。

``` bash
$ aaa
```

## 主题配置文件
