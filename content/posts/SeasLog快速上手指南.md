---
title: SeasLog快速上手指南
categories:
  - 开发笔记
tags:
  - SeasLog
date: 2017-12-10 13:08:22
---

## 什么是 SeasLog

log 日志，通常是系统或软件、应用的运行记录。通过 log 的分析，可以方便用户了解系统或软件、应用的运行情况；如果你的应用 log 足够丰富，也可以分析以往用户的操作行为、类型喜好、地域分布或其他更多信息；如果一个应用的 log 同时也分了多个级别，那么可以很轻易地分析得到该应用的健康状况，及时发现问题并快速定位、解决问题，补救损失。

[SeasLog](https://github.com/Neeke/SeasLog)是一个 C 语言编写的 PHP 扩展，提供一组规范标准的功能函数，在 PHP 项目中方便、规范、高效地写日志，以及快速地读取和查询日志。SeasLog 具备以下几个特点：

- 高性能

SeasLog 使用 C 语言编写，并带有缓冲池的功能。每次写入的日志，是先写入到内存当中，当达到一定的数量时，才写入到文件当中。

- 配置简单

SeasLog 的配置十分简单，甚至不需要配置就可以直接使用。

- 功能完善，使用简单

支持日志级别
支持日志分模块存放
支持日志统计，分析

## 如何安装 SeasLog

- SeasLog 源码下载地址：https://pecl.php.net/package/SeasLog

- SeasLog 源码安装过程（Linux）

```shell
# 下载源码
wget https://pecl.php.net/get/SeasLog-1.6.9.tgz

# 解压缩源码
tar -zxvf SeasLog-1.6.9.tgz

# phpize工具生源码编译文件
cd SeasLog-1.6.9
phpize

# 源码编译安装
./configure
make
make install

# 修改php.ini文件添加扩展
vi /path/to/php.ini
extension=seaslog.so

# 检查扩展是否安装成功
php -m | grep SeasLog
```

- 安装过程中可能会遇到的问题

```shell
# phpize需要php-devel依赖
Can't find PHP headers in /opt/rh/rh-php56/root/usr/include/php The php-devel package is required for use of this command.
解决办法：
1、先搜索此依赖
yum search php-devel
2、安装与当前版本兼容的依赖
yum install rh-php56-php-devel

# 查找php相关配置、扩展的位置
1、查看php命令行位置
which php
2、查看phpize命令行位置
which phpize
3、查看php.ini位置
php -i | grep Configuration
4、查看php模块位置
php -i | grep modules
5、查看php是否已安装某扩展
php -m | grep SeasLog
6、查看配置项的值
php -i | grep date.timezone
7、修改默认timezone
vi /path/to/php.ini
date.timezone = "Asia/Shanghai"

# 扩展无效的问题
doesn't appear to be a valid Zend extension
解决办法：
使用：extension=seaslog.so
不使用：zend_extension=seaslog.so
```

## SeasLog 如何记录日志

- 在 php 中写入日志

```php
&lt;?php

SeasLog::info('this is a seaslog info');
```

- 在日志目录查看日志

```shell
cd /data/log/default
cat 20171210.log

# 输出内容
info | 17416 | 1512886743.284 | 2017:12:10 14:19:03 | this is a seaslog info
```

## 参考资料

- [SeasLog Github 地址](https://github.com/Neeke/SeasLog)
- [使用 SeasLog 打造高性能日志系统](http://blog.csdn.net/u011120720/article/details/51474488)
- [【慕课网视频】高性能的 PHP 日志系统—SeasLog](http://www.imooc.com/learn/591)
