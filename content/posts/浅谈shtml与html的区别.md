---
title: 浅谈shtml与html的区别
tags:
  - shtml
id: 555
categories:
  - 开发笔记
date: 2016-10-14 10:04:29
---

> 遇到有人咨询 **_shtml_** 与 **_html_** 的区别，顺手查了一波资料，根据我的理解整理一下与大家分享。

## 何为shtml？

> shtml跟html类似，也是一种用于网页设计的标记型语言，区别在于：html是一种纯静态的标记型语言，在html文档里面写的内容是什么，用户打开浏览器看到的就是什么，而shtml是一种半静态半动态的标记型语言，在shtml里面可以包含SSI命令，当用户在浏览器浏览shtml文档的时候，里面包含的SSI命令会被解析，然后再呈现内容给用户。

## 何为SSI？

> SSI是为WEB服务器提供的一套命令，这些命令只要直接嵌入到HTML文档的注释内容之中即可。例如：
>   `<!--#include file="index.html"-->` `<!--#echo var="DOCUMENT_NAME"-->`都属于SSI指令。前者用于包含一个html文件，后者用于显示当前文档的名称。

## 举例说明

*   新建一个index.html文件，并输入代码：
```
    <!DOCTYPE HTML>
    <html>
    <head>
        <title>了不起的盖茨比</title>
        <meta charset="utf-8">
    </head>
    <body>
        <h1>了不起的盖茨比</h1>
        <p>1922年的春天，一个想要成名名叫<em>尼克。卡拉威</em>的作家，离开了美国中西部，来到了纽约。那是一个道德感缺失，爵士乐流行，走私为王，股票飞涨的时代。为了追寻他的<span>美国梦</span>，他搬入纽约附近一海湾居住。</p>
        <p>菲茨杰拉德，二十世纪美国文学巨匠之一，兼具作家和编剧双重身份。他以诗人的敏感和戏剧家的想象为<strong>“爵士乐时代”</strong>吟唱华丽挽歌，其诗人和梦想家的气质亦为那个奢靡年代的不二注解。</p>
    </body>
    </html>
```

    在浏览器访问该index.html文档，将会显示成这样：
    ![1](http://ov13triio.bkt.clouddn.com/2016/10/1.png)

*   新建一个test.shtml，并输入代码：

```
<!--#include file="index.html"-->
<!--#echo var="DOCUMENT_NAME"--> 
```

在浏览器访问该test.shtml，将会显示成这样：
![2](http://ov13triio.bkt.clouddn.com/2016/10/2.png)

可以看到上面显示的内容与index.html文件显示的内容完全相同，这也就是`<!--#include file="index.html"-->`这个SSI指令解析之后的结果，下面显示的test.shtml是`<!--#echo var="DOCUMENT_NAME"-->`这个SSI指令显示的内容。

*   两个文件的目录结构如下：
![3](http://ov13triio.bkt.clouddn.com/2016/10/3.png)

## 一点必要的说明

直接按照上面的演示去创建index.html和test.shtml两个文件，在浏览器访问test.shtml文档的时候会显示一片空白，这是因为shtml需要服务器配置支持SSI指令后方可解析其中的SSI指令。对于只需要了解html与shtml的区别的同学来说，没有必要再去深入研究。对于装了apache服务器的同学，可以按照下面的步骤去更改apache配置，让其能支持shtml。
1\. 打开apache的httpd.conf文件，搜索“AddType text/html .shtml”
2\. 去掉这两行前面的`#`注释
`# AddType text/html .shtml`
`# AddOutputFilter INCLUDES .shtml`
3\. 搜索`Options Indexes FollowSymLinks`并将其更改为`Options Indexes FollowSymLinks Includes`
4\. 保存httpd.conf，重启apache

## 总结

> html是纯静态标记语言，在里面写什么内容，浏览器就显示什么内容。shtml是半静态半动态标记语言，可以在里面包含SSI指令，配置服务器支持shtml之后，shtml文件里面的SSI指令会被解析，在浏览器浏览shtml文档，看到的是SSI指令被解析之后的结果。