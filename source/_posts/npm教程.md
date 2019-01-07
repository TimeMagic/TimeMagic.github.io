---
title: npm教程
password: 1
date: 2019-01-02 08:53:24
tags: npm
categories: npm
---

# npm教程

淘宝镜像

1.临时使用
npm --registry https://registry.npm.taobao.org install express


2.持久使用
npm config set registry https://registry.npm.taobao.org

配置后可通过下面方式来验证是否成功 
npm config get registry

或 

npm info express

3.通过cnpm使用
npm install -g cnpm --registry=https://registry.npm.taobao.org

使用 
cnpm install express


NPM代理


在创建ionic项目的时候报错：

Error: connect ECONNREFUSED 192.30.255.112:443

解决方案：设置你的全局代理

1.set https_proxy=http:代理：端口  eg:set https_proxy=http:10.172.115.85:2145(这是例如，具体设置成为自己的代理和端口)

2.set http_proxy=http：代理：端口 eg:set http_proxy=http:10.172.115.85:2145(这是例如，具体设置成为自己的代理和端口)

如果还是不行的话就再次给npm设置代理

为npm设置代理
$ npm config set proxy http://server:port
$ npm config set https-proxy http://server:port
如果代理需要认证的话可以这样来设置。
eg:npm config set proxy http://proxy.neusoft.com:8080

$ npm config set proxy http://username:password@server:port
$ npm config set https-proxy http://username:pawword@server:port
如果代理不支持https的话需要修改npm存放package的网站地址。
eg:npm config set proxy http://yang.mq:Ymq@1026@proxy.neusoft.com:8080

$ npm config set registry "http://registry.npmjs.org/"

如果还是不行的话就360强力干掉npm重装npm

config生成的文件位置在   c:盘 --》 用户(folder)

去掉代理

$ npm config delete proxy

$ npm config delete https-proxy




npm ERR! code EINTEGRITY npm ERR! sha1-
报错日志
npm ERR! code EINTEGRITY
npm ERR! sha1-OGchPo3Xm/Ho8jAMDPwe+xgsDfE= integrity checksum failed when using sha1: wanted sha1-OGchPo3Xm/Ho8jAMDPwe+xgsDfE= but got sha1-gNVXCrjQagTW0VaF+kYHiU1O0Iw=. (33078 bytes)
解决办法
系统环境信息

node v9.2.0
npm  v5.5.1
清除缓存 尝试失败
npm cache verify

npm cache clean --force
2.把npm版本降级为4系最新版本，解决

npm -g install npm@4



1.如果有package-lock.json文件，就删掉

2.管理员权限进入cmd

3.执行npm cache clean --force

4.之后再npm install

有时候网不好也会出现问题，多试几次。

npm cache verify
npm cache clean
npm cache clean --force
npm i -g npm
grep -ir "sha1-xxxxxxxxxxxxxxxx" ~/.npm


对于上面的问题，根本原因在于package-lock.json
npm升级到5版本以后，添加了pacgage-lock.json文件，作用在于锁定版本号，x.y.z以便统一版本
出现问题的原因就是因为package-lock.json的包的校验文件没有匹配上。
解决问题的办法就是，通过npm install xxx（包）重新匹配上
