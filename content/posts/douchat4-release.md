---
title: douchat 4.0 新版发布，助力小程序后台开发
categories:
  - 开源项目
tags:
  - 微信开发
  - douchat
date: 2018-05-08 16:50:37
author: 
  name: 艾豆工作室
  link: http://idoustudio.com
---

## douchat是什么

[douchat](http://douchat.net)（中文名：豆信）是一款专为微信开发而创造的php开源框架，具有简洁、高效、优雅等特点。

## 新版本功能

douchat V4.0.0版本主要新增了小程序开发支持，具体包括：

- 插件新增Api控制器，可快速实现供小程序调用的接口
- 封装了小程序登录态管理逻辑，可以更快捷的实现小程序端用户登录逻辑
- 框架后台新增了小程序账号管理、小程序用户管理等功能
- 新增Wxapps目录，并添加了一个自带的Demo小程序模板

## 主要功能介绍

### 多账户管理

豆信后台支持创建多个用户，每个登录用户可以创建多个公众号和小程序账号。
![](http://blogcdn.idoustudio.com/douchat4-release-1.png)

### 多应用管理

豆信支持通过插件的形式来扩展系统的功能，每一个插件可以用于公众号h5页面访问，也可以为小程序应用提供接口。
![](http://blogcdn.idoustudio.com/douchat4-release-2.png)

### 开发友好

豆信封装了诸多对开发者友好的业务逻辑，让开发者能够更快速的实现自己的应用功能。

- 视图工具

豆信封装的视图工具可以让开发者通过简单的几行代码快速实现管理后台对数据的增删改查操作。

![](http://blogcdn.idoustudio.com/douchat4-release-3.png)

- 公众号功能封装

豆信封装了常用的公众号开发逻辑，比如微信支付、发送模板消息、企业付款等。使用豆信封装的逻辑进行开发，可以大大简化开发者的工作量。
![](http://blogcdn.idoustudio.com/douchat4-release-4.png)

- 小程序开发支持

豆信对小程序的登录态管理、用户信息更新等逻辑做了封装。
![](http://blogcdn.idoustudio.com/douchat4-release-5.png)

- 应用商城

![](http://blogcdn.idoustudio.com/douchat4-release-6.png)


## 相关资源

- 产品官网：[http://douchat.net](http://douchat.net)
- 代码仓库：[https://github.com/mikemintang/douchat.git](https://github.com/mikemintang/douchat.git)
