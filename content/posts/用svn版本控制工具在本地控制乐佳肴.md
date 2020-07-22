---
title: 用SVN版本管理工具在本地对项目代码进行管理
id: 341
categories:
  - 开发笔记
date: 2014-10-21 11:50:02
tags:
  - svn
  - 版本控制
---

对于一个程序开发人员来说，对代码的版本控制毫无疑问是一个非常关键的工作。因为一直都是一个人在写程序做产品，没有团队协作的概念，所以我以前做一些产品的时候基本上就是有了一个想法立马就去开始写程序了。没有写过需求文档，没有进行过市场调研，更没有规范化的开发进度控制、代码管理等，因此我以前做过的“武大微淘”、“凌波微购”、“厚德网”等产品的完整源代码现在都找不到了，有时候想起来还是觉得挺悲伤的。就拿“武大微淘”来说，我记得我是从2014年2月15号，也就是大三下学期刚开学开始，每天都去图书馆写程序，大概写了半个月才把“武大微淘”的基本功能写完，那时候的功能想想还是很不错的，虽然做的也是一个微信电商平台，但“武大微淘”相比于其他的微信购物类产品而言还是有很多亮点，比如说支持用户直接在微信中回复关键词查询商品分类、商品列表等，还有就是通过回复“商品编号+份数+地址”可以在微信中直接提交订单、管理员输入“查询+订单号”直接在微信中查看订单等。

如果可以的话我还是希望 能把以前做的一些产品的源代码找到，稍微修改一下还能继续使用，指不定还能卖给某些感兴趣的人。可惜的是以前图样图森破，不懂得对代码进行管理，导致现在想找都很难找到了。因此，以后开发什么产品一定要学会用代码管理工具，把每个版本的代码完整的保留下来。

ok。现在开始进入正题。分享如何在本地配置SVN。

准备工作：

下载并安装svn服务器：[http://www.visualsvn.com/server/download/](http://www.visualsvn.com/server/download/)

下载并安装svn客户端：[http://tortoisesvn.net/downloads.html](http://tortoisesvn.net/downloads.html)

与SVN配置相关的教程：
[http://www.cnblogs.com/elesos/archive/2012/12/09/2810449.html](http://www.cnblogs.com/elesos/archive/2012/12/09/2810449.html)
[http://www.cnblogs.com/skyway/archive/2011/08/10/2133399.html](http://www.cnblogs.com/skyway/archive/2011/08/10/2133399.html)

本地配置SVN

1、在任意目录下新建SVN仓库
![](http://ov13triio.bkt.clouddn.com/2014/10/1.jpg)

2、复制仓库地址
![](http://ov13triio.bkt.clouddn.com/2014/10/2.jpg)


3、SVN仓库创建成功
![](http://ov13triio.bkt.clouddn.com/2014/10/3.jpg)

4、选择需要导入的项目文件夹
![](http://ov13triio.bkt.clouddn.com/2014/10/4.jpg)

5、填写SVN仓库地址和项目版本信息
![](http://ov13triio.bkt.clouddn.com/2014/10/5.jpg)

6、导入项目至SVN仓库
![](http://ov13triio.bkt.clouddn.com/2014/10/6.jpg)

7、从SVN仓库到处项目至新的文件夹
![](http://ov13triio.bkt.clouddn.com/2014/10/7.jpg)

8、填写SVN仓库地址及导出到的路径
![](http://ov13triio.bkt.clouddn.com/2014/10/8.jpg)

9、导出项目源代码
![](http://ov13triio.bkt.clouddn.com/2014/10/9.jpg)

10、用SVN进行管理
![](http://ov13triio.bkt.clouddn.com/2014/10/10.jpg)