---
title: 留言板插件：Liuyanban
tags:
  - weiphp
  - weiphp插件开发
id: 444
categories:
  - 系列分享
date: 2015-01-10 00:53:26
---

现在市面上有很多形如《xxx 实战教程》、《xxx 案例开发大全》之类的开发书籍，关于书的内容质量怎么样我就不多说了，呵呵一句你们就会懂的。我个人还是比较喜欢国外的编程教学类书籍，书中讲解的内容大部分都是比较严谨的，不仅讲实战，也会把原理解释得很清楚。但是国内的教程能做到这样的就很少了。回到正题上来，我们这次讲解的是 weiphp 留言板插件的开发流程，大家都知道，留言板是一门 web 开发技术入门级的案例，学习任何一门 web 开发技术，不管是 java、php 还是 python、ruby 之类的，要是能够写出一个留言板的实例来，那么就可以证明你算是入门了。所以我们今天就开始通过一个留言板的实例来入门 weiphp 插件开发。

在开发此插件前，我们先来看一下这个插件的效果图，这里给大家分享一个我用 bootstrap 写的比较简单的留言板界面，一共两个页面：留言列表页面和发布留言页面，大家可以在我的百度云上面下载这个前台模板：[http://pan.baidu.com/s/1sjFgsVF](http://pan.baidu.com/s/1sjFgsVF)

留言列表页面

![](http://blogcdn.idoustudio.com/2015/01/0.1.png)

发布留言页面

![](http://blogcdn.idoustudio.com/2015/01/0.2.png)

看到前台效果图，我们大概知道这个插件做好之后是怎么样了，这个插件相对来说比较简单，需要用到 weiphp 插件模板、数据模型等。

下面我们开始一步一步来完成这个插件：

1、创建插件。在 weiphp 管理后台中创建留言板插件，把下面的安装后是否立即启用、是否需要配置、是否需要管理列表全部勾上（如果需要进行数据库操作，并且插件用到的数据库只有一个表时就勾上需要管理列表，这时候在插件管理后台就会出现数据管理列表，列表里面的内容默认会调用与插件标识名一致的数据模型进行显示，也就是说如果我们把是否需要管理列表这个选项勾上了，那么我们在创建数据模型的时候需要把数据模型命名为 Liuyanban，这样的话我们就不需要对数据模型显示的方法进行重载即可调用 weiphp 默认的方法进行数据显示与管理，如果我们的模型名与插件标识名不一致的话，那么我们需要改写数据管理控制器，这个后面会讲到。）

![](http://blogcdn.idoustudio.com/2015/01/1.1.png)

![](http://blogcdn.idoustudio.com/2015/01/1.2.png)

2、安装插件。

![](http://blogcdn.idoustudio.com/2015/01/23.png)

3、查看插件是否安装成功。在插件管理后台可以看到留言板插件已经成功安装了，因为勾选了需要配置和需要管理列表，所以这个地方能看到配置页面和数据管理页面，因为数据模型和数据列表还没有定义，所以数据管理页面并没有显示每个字段标题。

![](http://blogcdn.idoustudio.com/2015/01/34.png)

4、创建数据模型。在 weiphp 管理后台依次点击“系统->模型管理->新增”，进入数据模型创建页面，并创建 Liuyanban 数据模型。

![](http://blogcdn.idoustudio.com/2015/01/42.png)

5、创建字段。在模型列表里面找到上一步创建好的 Liuyanban 数据模型，并点字段管理进入字段管理页面。

![](http://blogcdn.idoustudio.com/2015/01/53.png)

点击新增依次建立如下字段。

![](http://blogcdn.idoustudio.com/2015/01/5.2.png)

6、定义数据列表。在模型列表页面再次找到 liuyanban 模型，点击编辑进入模型编辑页面；

![](http://blogcdn.idoustudio.com/2015/01/6.1.png)

在模型编辑页面点击设计进入列表定义页面；

![](http://blogcdn.idoustudio.com/2015/01/6.2.png)

按下图所示进行列表定义。

![](http://blogcdn.idoustudio.com/2015/01/6.3.png)

7、查看数据列表。再次回到插件管理后台，可以看到列表定义的字段已经显示出来。

![](http://blogcdn.idoustudio.com/2015/01/72.png)

8、编辑配置文件。改写 config.php，添加下图所示配置项。

![](http://blogcdn.idoustudio.com/2015/01/81.png)

回到插件管理后台，可以看到配置项成功应用。

![](http://blogcdn.idoustudio.com/2015/01/8.2.png)

上传一张封面图片并点“确定”提交配置项。

![](http://blogcdn.idoustudio.com/2015/01/8.3.png)

9、编写微信交互模型。在微信模型中设置当用户输入“留言板”触发此插件时回复单图文消息，跳转到留言列表页面（Liuyanban 控制器下的 index 方法），并传递用户的 openid 和公众号的 token 两个参数。其中 get_token()、get_openid()、addons_url()、get_cover_url()为 weiphp 封装的较为常用的方法，可以在 Application/Common/Common/function.php 中查看这些方法对应的用法和功能。

![](http://blogcdn.idoustudio.com/2015/01/91.png)

10、显示前台模板。在 Liuyanban 控制器中新建 index()和 liuyan()两个方法，分别用于显示留言列表页面和发布留言页面。

![](http://blogcdn.idoustudio.com/2015/01/9.1.png)

下载前台模板文件[http://pan.baidu.com/s/1sjFgsVF](http://pan.baidu.com/s/1sjFgsVF)，并把文件粘贴至 Liuyanban 插件的模板文件夹下面。

![](http://blogcdn.idoustudio.com/2015/01/10.2.png)

11、微信调试。在微信中回复“留言板”即可响应此插件回复单图文消息。

![](http://blogcdn.idoustudio.com/2015/01/11.1.png)

点击即可进入留言列表页面。

![](http://blogcdn.idoustudio.com/2015/01/11.2.png)

打开 index.html，用 U 方法为“写留言”添加跳转链接，使其被点击后跳转到发布留言页面。

![](http://blogcdn.idoustudio.com/2015/01/11.3.png)

因此在留言列表页中点击“写留言”时会跳转到下图所以发布留言页面。

![](http://blogcdn.idoustudio.com/2015/01/11.5.png)

打开 liuyan.html，用 U 方法为“看留言”添加跳转链接，使其被点击后跳转到留言列表页面。

![](http://blogcdn.idoustudio.com/2015/01/11.4.png)

经过以上几步完成了两个页面之间的相互跳转。

12、发布留言。在 liuyan.html 页面中查看 form 表单中的 input 的 name。

![](http://blogcdn.idoustudio.com/2015/01/12.1.png)

根据 name 编辑 liuyan 方法，把留言数据插入到数据模型。

![](http://blogcdn.idoustudio.com/2015/01/12.2.png)

13、发布留言。在发布留言页面插入几条留言。

![](http://blogcdn.idoustudio.com/2015/01/13.1.png)

在插件管理后台可以看到所有用户发布的留言。

![](http://blogcdn.idoustudio.com/2015/01/13.2.png)

13、编辑留言。在编辑留言的页面添加管理员回复内容。

![](http://blogcdn.idoustudio.com/2015/01/14.1.png)

15、显示留言。编辑 index 方法，取出数据模型中的留言数据并赋值给变量。

![](http://blogcdn.idoustudio.com/2015/01/15.1.png)

用 volist 标签循环输出留言数据。

![](http://blogcdn.idoustudio.com/2015/01/161.png)

16、查看留言。可以看到所有的留言数据都显示出来，至此本插件开发完毕。

![](http://blogcdn.idoustudio.com/2015/01/151.png)
