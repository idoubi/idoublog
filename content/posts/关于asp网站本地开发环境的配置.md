---
title: 关于asp网站本地开发环境的配置
tags:
  - asp
  - iis
id: 172
categories:
  - 开发笔记
date: 2014-01-20 22:49:29
---

采用不同的语言开发网站所需要的开发环境也会有所不同，php 网站开发环境需要 apache 作为服务器，一般用 mysql 数据库，对于 php 的开发环境配置一般人会选择集成环境包，比如 wampserver 之类的，可以省去很多力气。jsp 网站需要 tomcat 作为服务器，一般用 mysql 或者 sql server 数据库，开发环境的配置需要配置环境变量什么的，过程有点繁琐。而 asp 网站一般是用 IIS 作为服务器，windows 系统一般自带 IIS 服务，但是开发环境需要自己配置。

下面主要讲一下 asp 开发环境的配置步骤。

1、打开或关闭 windows 功能

单击 开始->控制面板->程序和功能->打开或关闭 windows 功能

在 internet 信息服务那个选项上打勾（把下面的子目录也全部勾上）
![](http://blogcdn.idoustudio.com/2014/01/1.png)

然后点确定，过几分钟就好了。

2、配置 IIS 管理器

单击 开始->控制面板->管理工具->Internet 信息服务(IIS)管理器

打开 Internet 信息服务(IIS)管理器面板
![](http://blogcdn.idoustudio.com/2014/01/2.png)

接下来再经过四步设置就完成了 asp 开发环境的配置

a、双击 Internet 信息服务(IIS)管理器面板中的 ASP，进行两处配置，一处是配置将错误发送到浏览器为 true，第二处是启用父路径设置为 true
![](http://blogcdn.idoustudio.com/2014/01/3.png)

![](http://blogcdn.idoustudio.com/2014/01/4.png)
![](http://blogcdn.idoustudio.com/2014/01/5.png)

b、双击默认文档，添加一个 index.asp 文档。
![](http://blogcdn.idoustudio.com/2014/01/7.png)

![](http://blogcdn.idoustudio.com/2014/01/6.png)

c、双击高级设置，选择 IIS 根目录对应的物理路径。
![](http://blogcdn.idoustudio.com/2014/01/8.png)

d、单击绑定，为 IIS 网站绑定主机名和端口。（默认端口为 80，如果电脑上配置了其他服务器比如 apache、tomcat 之类的，为了防止端口冲突，可以在这里给 IIS 网站配置一个端口。我平常一般 asp 网站和 php 网站一起开发，所以在这里给 asp 网站设置端口为 81，php 那里设置为默认的 80）
![](http://blogcdn.idoustudio.com/2014/01/9.png)

经过上面四个步骤，基本上 IIS 配置就差不多了，如果你的电脑是 64 位系统，还需要进行一处很重要的配置（妈蛋今天因为忘记配置这个了，搞得我的 asp 网站半天打不开。。。）

e、双击左侧的应用程序池，在 DefaultAppPool 上右键，选择高级设置，启用 32 位应用程序。（如果是 32 位系统可以省略这一步）
![](http://blogcdn.idoustudio.com/2014/01/10.png)

ok，IIS 配置完成，现在可以 asp 网站了，下面讲一下怎么在 dreamweaver 中给站点建立测试服务器和远程服务器

3、dreamweaver 中几种服务器的配置

a、配置本地服务器
![](http://blogcdn.idoustudio.com/2014/01/12.png)

b、配置远程服务器
![](http://blogcdn.idoustudio.com/2014/01/11.png)

c、配置 svn 服务器
![](http://blogcdn.idoustudio.com/2014/01/14.png)

svn 版本控制配置
![](http://blogcdn.idoustudio.com/2014/01/15.png)
