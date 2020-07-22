---
title: 第一个自定义weiphp插件：MyHello
tags:
  - weiphp
  - weiphp插件开发
id: 405
categories:
  - 系列分享
date: 2015-01-05 15:02:54
---

我们都知道，学习任何一门编程语言，一般来说第一个范例程序就是输出“Hello World”。从今天开始我们来学习 weiphp 插件开发，也从一个 HelloWorld 级别的插件开始讲起，因为安装好了的 weiphp 框架，默认安装了 weiphp 官方开发的 HelloWorld 插件，所以这里为了防止跟官方的插件冲突，我们开发的第一个自定义 weiphp 插件取名为 MyHello，并通过这个简单的插件来讲解 weiphp 插件开发的整个流程。整个【分分钟上手 weiphp 插件开发】系列教程都是直接从创建一个新的插件开始讲起的，逐步延生到插件编码、微信响应、数据模型设计等内容，并假定阅读本教程的用户都具有一定的 php 编程基础且已成功搭建好了 weiphp 开发框架，关于如何在服务器上部署 weiphp、weiphp 框架代码结构、weiphp 数据模型等知识请自行参考其他的教程。本系列教程基于的 weiphp 框架版本为 2.0.1202，依赖的服务器为阿里云。

下面开始讲解第一个自定义 weiphp 插件：MyHello 的开发流程。

1、插件创建。在 weiphp 管理后台依次点击“插件管理->创建插件”进入插件创建页面，填写插件的标识名、插件名、版本、作者、描述等信息，勾选“安装后是否启用”、“是否需要配置”两项，点击“确定”完成插件的创建。

![](http://blogcdn.idoustudio.com/2015/01/11.png)
![](http://blogcdn.idoustudio.com/2015/01/21.png)

2、插件安装。在插件管理列表中点击“安装”完成插件的安装。

![](http://blogcdn.idoustudio.com/2015/01/32.png)

3、插件管理。返回到 weiphp 管理前台，可以看到 MyHello 插件已经成功安装。

![](http://blogcdn.idoustudio.com/2015/01/41.png)

4、改写配置文件。在 weiphp 的 addons 目录下默认生成的 MyHello 插件文件夹下面改写默认生成的 config.php，添加如下图所示配置项。

![](http://blogcdn.idoustudio.com/2015/01/51.png)

5、查看配置项。可以看到配置文件已经正常响应。

![](http://blogcdn.idoustudio.com/2015/01/61.png)

6、微信响应。为 WeixinAddonModel.class.php 中编写微信响应代码。

![](http://blogcdn.idoustudio.com/2015/01/71.png)

7、编辑配置项。在后台配置页面填写配置信息，上传封面图片，并点“确定”提交配置项。

![](http://blogcdn.idoustudio.com/2015/01/8.png)

8、微信测试。在微信中回复“我的插件”或者“MyHello”时，根据配置项中选择的回复类型是“文本消息”还是“单图文消息”来进行回复。

![](http://blogcdn.idoustudio.com/2015/01/9.png)
