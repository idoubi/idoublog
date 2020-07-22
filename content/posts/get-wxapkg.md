---
title: 小程序源码反编译实战笔记
categories:
  - 开发笔记
tags:
  - 小程序
  - 反编译
date: 2018-05-31 16:50:37
author: 
  name: 艾逗笔
  link: http://idoubi.cc
---

最近在做微信小程序开发，看到一些做的比较有意思的小程序，想看一看他们的源码，于是研究了一下小程序源码反编译相关的技术。结合本次经历，总结如下。


## 手机root

要拿到小程序在手机上的源码包，需要有一台越狱的iphone或者一台拥有root权限的android机，正好我手里有一台闲置的小米4，就拿来用了，试过很多种方式给小米4root，差点搞成板砖机，一直在recorvy模式启动不了，最后尝试了小米官网的刷机方法，把系统刷成了开发版，完美root。

小米4刷机教程：[https://www.miui.com/shuaji-305.html](https://www.miui.com/shuaji-305.html)

手机刷机成功后，开启root权限，并打开USB调试模式

小米4开发版开启root权限：[http://www.miui.com/thread-9625466-1-1.html](http://www.miui.com/thread-9625466-1-1.html)

MIUI9开启USB调试：[https://jingyan.baidu.com/article/49711c6196e728fa441b7c37.html](https://jingyan.baidu.com/article/49711c6196e728fa441b7c37.html)

## 电脑操作

以`MacOS`操作系统为例，通过USB连接操作Android手机

- 安装`adb`工具

在`MacOS`系统上可以通过`brew`安装`adb`工具

```
brew cask install android-platform-tools
```

- 手机连电脑

通过USB数据线将手机连接到电脑，并在手机端开启USB调试，通过下面的命令测试是否连接成功

```
adb devices
```

连接成功的情况下

![](http://blogcdn.idoustudio.com/wxa_1.png)

- 查看小程序源码

进入`adb shell`模式
```
adb shell
```

切换到root权限
```
su
```

进入小程序源码目录

先进入`MicroMsg`文件夹，通过`ls`命令查看文件夹，找到`63c92a20722afef36b525ecb04706c15`这样的文件夹（不同的微信登录用户，这个文件夹的名称不同），然后再进入`appbrand/pkg`目录
```
cd /data/data/com.tencent.mm/MicroMsg
ls
cd 63c92a20722afef36b525ecb04706c15/appbrand/pkg
```

查看小程序源码

可以先执行`rm -rf ./*`删除掉当前目录下所有缓存的小程序源码包，然后通过手机端点击进入需要获取源码的小程序，再执行`ls -l`即可看到最新操作的小程序源码包
```
rm -rf ./*
ls -l
```

将小程序源码拷贝到手机SD卡
```
cp _1038319936_4.wxapkg /sdcard
```

新开一个终端窗口，将源码拷贝到电脑

```
adb pull sdcard/_1038319936_4.wxapkg /data/weapp/a.wxapkg
```

- 操作过程截图

![](http://p7vlrarrm.bkt.clouddn.com/wxa_2.png)

![](http://p7vlrarrm.bkt.clouddn.com/wxa_3.png)

- 反编译源码

下载`nodejs`版本的反编译工具并安装相关`npm`包
```
git clone https://github.com/qwerty472123/wxappUnpacker.git

cd wxappUnpacker

npm install esprima -g
npm install css-tree -g
npm install cssbeautify -g
npm install vm2 -g
npm install uglify-es -g

```

反编译小程序源码
```
node wuWxapkg.js /data/weapp/a.wxapkg
```

最后进入反编译成功的文件，即可看到需要的小程序文件
![](http://p7vlrarrm.bkt.clouddn.com/wxa_4.png)

## 参考文章

只需两步获取任何微信小程序源码：[https://juejin.im/post/5b0e431f51882515497d979f](https://juejin.im/post/5b0e431f51882515497d979f)
微信小程序“反编译”实战（一）：解包：[https://juejin.im/post/5af17b72518825383611ac38](https://juejin.im/post/5af17b72518825383611ac38)