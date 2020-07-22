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

采用不同的语言开发网站所需要的开发环境也会有所不同，php网站开发环境需要apache作为服务器，一般用mysql数据库，对于php的开发环境配置一般人会选择集成环境包，比如wampserver之类的，可以省去很多力气。jsp网站需要tomcat作为服务器，一般用mysql或者sql server数据库，开发环境的配置需要配置环境变量什么的，过程有点繁琐。而asp网站一般是用IIS作为服务器，windows系统一般自带IIS服务，但是开发环境需要自己配置。

下面主要讲一下asp开发环境的配置步骤。

1、打开或关闭windows功能

单击 开始->控制面板->程序和功能->打开或关闭windows功能

在internet信息服务那个选项上打勾（把下面的子目录也全部勾上）
![](http://ov13triio.bkt.clouddn.com/2014/01/1.png)

然后点确定，过几分钟就好了。

2、配置IIS管理器

单击 开始->控制面板->管理工具->Internet 信息服务(IIS)管理器

打开Internet 信息服务(IIS)管理器面板
![](http://ov13triio.bkt.clouddn.com/2014/01/2.png)

接下来再经过四步设置就完成了asp开发环境的配置


a、双击Internet 信息服务(IIS)管理器面板中的ASP，进行两处配置，一处是配置将错误发送到浏览器为true，第二处是启用父路径设置为true
![](http://ov13triio.bkt.clouddn.com/2014/01/3.png)

![](http://ov13triio.bkt.clouddn.com/2014/01/4.png)
![](http://ov13triio.bkt.clouddn.com/2014/01/5.png)

b、双击默认文档，添加一个index.asp文档。
![](http://ov13triio.bkt.clouddn.com/2014/01/7.png)

![](http://ov13triio.bkt.clouddn.com/2014/01/6.png)

c、双击高级设置，选择IIS根目录对应的物理路径。
![](http://ov13triio.bkt.clouddn.com/2014/01/8.png)

d、单击绑定，为IIS网站绑定主机名和端口。（默认端口为80，如果电脑上配置了其他服务器比如apache、tomcat之类的，为了防止端口冲突，可以在这里给IIS网站配置一个端口。我平常一般asp网站和php网站一起开发，所以在这里给asp网站设置端口为81，php那里设置为默认的80）
![](http://ov13triio.bkt.clouddn.com/2014/01/9.png)

经过上面四个步骤，基本上IIS配置就差不多了，如果你的电脑是64位系统，还需要进行一处很重要的配置（妈蛋今天因为忘记配置这个了，搞得我的asp网站半天打不开。。。）

e、双击左侧的应用程序池，在DefaultAppPool上右键，选择高级设置，启用32位应用程序。（如果是32位系统可以省略这一步）
![](http://ov13triio.bkt.clouddn.com/2014/01/10.png)

ok，IIS配置完成，现在可以asp网站了，下面讲一下怎么在dreamweaver中给站点建立测试服务器和远程服务器

3、dreamweaver中几种服务器的配置

a、配置本地服务器
![](http://ov13triio.bkt.clouddn.com/2014/01/12.png)

b、配置远程服务器
![](http://ov13triio.bkt.clouddn.com/2014/01/11.png)

c、配置svn服务器
![](http://ov13triio.bkt.clouddn.com/2014/01/14.png)

svn版本控制配置
![](http://ov13triio.bkt.clouddn.com/2014/01/15.png)
