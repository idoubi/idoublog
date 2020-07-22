---
title: 开发一个自己的composer包
tags:
  - composer
  - php
id: 798
categories:
  - 开发笔记
date: 2017-08-21 17:04:20
---

> php的composer类似于node的包管理机制，都是可以通过一些命令操作下载开发者发布的功能包，并且包之间可以互相依赖，管理起来比较方便。下面根据自己踩过的坑总结一下如何创建一个自己的composer包。

## 创建一个composer包

在packages目录下面创建一个自己的composer包：mikephp-db，composer.json里面填写包的基本信息，src目录下面是包的功能代码。

![](http://blogcdn.idoustudio.com/1.png)

composer.json的内容

```json
{
	"name": "mikephp/db",
	"description": "this is a libriary for db oprate",
	"autoload": {
		"psr-4": {
			"Db\\": "src/"
		}
	}
}
```

> 包的名称叫：mikephp/db，一旦包发布到网上，其他开发者就可以通过`composer require mikephp/db`来下载使用这个包。autoload配置使用psr-4自动加载规范，把`Db`命名空间映射到包的`src`目录下

Mysql.php的内容

```php
&lt;?php  

namespace Db;

class Mysql {

	public function __construct() {
		echo 'this is mysql db';
	}
}
```

> 包的src下的功能文件都应该使用命令空间，如果功能文件在src的一级目录，例如`src/Mysql.php`，命名空间是Db，如果是二级目录，例如`src/Pdo/Mysql.php`，那么命名空间就应该是`Db\Pdo`，在使用功能类的时候就通过`new Db\Mysql()`或者`Db\Pdo\Mysql()`使用

## 开发过程中调试创建的composer包

在把自己创建的composer包发布到网上供其他开发者使用前，我们需要不断调试以验证包的功能是否能正常使用。可以在项目不依赖composer包的前提下，通过配置autoload自动加载来关联开发中的composer包。下面的例子以composer为项目根目录，需要使用packages里面的正在开发的包。composer.json文件里面填写此项目需要依赖或者关联的包，如果有配置require，则在执行`composer install`或者`composer update`的时候，被依赖的包会被下载到vendor目录下。此处演示在项目根目录下通过autoload自动加载关联开发中的composer包，此情况下被关联的包不会被下载到vendor下。

![](http://blogcdn.idoustudio.com/2.png)

composer.json的内容

```json
{
	"require": {
	},
	"autoload": {
		"psr-4": {
			"Db\\": "packages/mikephp-db/src/"
		}
	}
}
```

> 使用psr-4自动加载规范，把Db命名空间映射到自己创建的composer包的src目录下

改好composer.json后执行一项`composer update`，此操作只会更新vendor目录下的autoload.php，不会下载包到vendor目录。

在测试文件中调试自己的composer包
test.php的内容

```php
&lt;?php  

require 'vendor/autoload.php';

$mysql = new Db\Mysql();
```

运行test.php

![](http://blogcdn.idoustudio.com/3.png)

## 为项目添加需要依赖的composer包

在composer包开发完成后，就可以在项目的composer.json中配置require来创建依赖，此种情况下通过`composer install`或者`composer update`会把包下载到vendor目录下

1、本地下载
在把已开发完成的composer包发布到网上前，可以通过配置repositories来依赖包。

修改根目录下的composer.json

```json
{
	"require": {
		"mikephp/db": "dev-master"
	},
	"repositories": [
		{
			"type": "path",
			"url": "packages/mikephp-db"
		}
	]
}
```
使用composer安装包

```php
composer update
```

使用`composer update`之后的目录

![](http://blogcdn.idoustudio.com/4.png)

> 使用本地依赖的方式，composer包会被下载到vendor目录，但是包里面是没有内容的，只是通过软连接的方式关联到了本地包的实际位置。

2、Github下载

可以把开发完成的composer包上传到[github](https://github.com)，然后配置项目的repository进行下载
修改composer.json

```json
{
	"require": {
		"mikephp/db": "dev-master"
	},
	"repositories": [
		{
			"type": "git",
			"url": "https://github.com/mikemintang/db.git"
		}
	]
}
```
使用`composer update`之后的目录

![](http://blogcdn.idoustudio.com/5.png)

3、Packagist加载

可以把开发完成的composer包发布到[Packagist](https://packagist.org)，在使用composer下载依赖包的时候默认会在Packagist搜索包并下载。

```json
{
	"require": {
		"mikephp/db": "dev-master"
	}
}
```
使用`composer update`之后的目录

![](http://blogcdn.idoustudio.com/6.png)

## 遇到的坑

在配置repositories下载composer包的过程中可能会遇到下图所示的报错信息，这种情况下要检查项目的composer.json填写的包的信息是否有误，version配置项要使用`dev-master`。应该是包的版本设置问题，需要好好研究一下composer包的版本设置规范。
![](http://blogcdn.idoustudio.com/7.png)

## 总结

本文根据一个实际的例子简要总结了如何开发自己的composer包并在项目中调试或使用。composer对于php而已是一个非常有利的工具，弥补了php在原生没有包管理机制上的不足。在项目开发过程中我们可以通过使用一些开源的composer包来提高开发效率，也可以把自己在项目开发者比较经常用到的一些技术或者工具类封装成composer包，发布到网上供其他开发者使用。

composer开发相关的一些资料或链接：

- 官网：https://getcomposer.org/
- 中文文档：http://docs.phpcomposer.com/
- 国内镜像：https://pkg.phpcomposer.com/
- 包管理平台：https://packagist.org/