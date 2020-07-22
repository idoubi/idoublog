---
title: 签到插件：QianDao
tags:
  - weiphp
  - weiphp插件开发
id: 509
categories:
  - 系列分享
date: 2015-04-12 20:30:35
---

相信看过《分分钟上手 weiphp 插件开发》系列教程 前三篇的朋友对 weiphp 插件开发应该不陌生了，那么今天就再来分享一个相对较为简单的小功能的开发过程：weiphp 签到插件开发详解。

其实签到功能在提高公众号用户粘性方面起到了很大的作用，通常会在一些电商类的平台中使用，运营人员通过设置相应的奖励来提高用户的签到积极性。鉴于时间有限，今天我要分享的这个签到功能较为简单，功能比较单一，感兴趣的朋友可以在此基础上继续扩展，完成更高级的功能，以达到商用的目标。

1、首先肯定是在 weiphp 超级管理后台创建这个插件。不需要配置，需要管理列表。

![](http://blogcdn.idoustudio.com/2015/04/1.png)

2、然后是安装这个插件。

![](http://blogcdn.idoustudio.com/2015/04/2.png)

3、创建一个名为 qiandao 的数据模型，并新建四个字段。

![](http://blogcdn.idoustudio.com/2015/04/3.png)

4、编辑 qiandao 数据模型的列表定义。

![](http://blogcdn.idoustudio.com/2015/04/4.png)

5、编辑微信交互模型。weiphp/Addons/QianDao/Model/WeixinAddonModel.class.php

![](http://blogcdn.idoustudio.com/2015/04/51.png)

代码解析：红框里面的就是整个签到功能的微信交互代码。首先获取到发送”签到“的用户的 openid 和当前公众号的 token，以及发送消息的时间和日期；然后判断一下该用户对应的 openid、token 以及日期 date 是否存在于签到表中（签到表中的不能存在 openid、token、date 完全相同的两条记录，即同一用户不能在同一天重复签到），如果用户已经签到过了，则提示用户不能重复签到。如果用户在发送消息的这一天是第一次签到，则把用户的 openid、当前公众号的 token、发送消息这天的日期 date 以及发送消息的时间 ctime 都存入到 qiandao 表，然后统计出签到表中日期 date 与用户发送消息当天的 date 相同的记录总数，提示用户是第几个签到的。

6、在插件管理页面就能看到用户的签到记录。

![](http://blogcdn.idoustudio.com/2015/04/7.png)

7、在微信端预览效果。

![](http://blogcdn.idoustudio.com/2015/04/61.png)

至此，签到功能开发完毕，有兴趣的朋友可以在此基础上扩展这个功能，比如管理员可以在后台设置签到规则和签到的奖品之类的，授人以鱼不如授人以渔，希望大家多多思考，能够掌握 weiphp 插件开发的精髓。
