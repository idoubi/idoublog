---
title: SQL2Struct：一款根据sql语句自动生成golang结构体的chrome插件
categories:
  - 开源项目
tags:
  - null
date: 2017-11-06 10:47:44
---

## 前言

最近在用 golang 写 api，用到 gorm 包进行数据库操作，[gorm](https://github.com/jinzhu/gorm)是 golang 中非常流行的一个 orm 包，使用 gorm 进行数据库操作前，一般需要先用一个 golang 结构体对数据表字段进行映射，于是我们经常需要根据数据表中的字段名和类型来手动在 go 代码中写 struct，有时候数据表字段很多的情况下，这种方式很耗费精力。于是便想到了写一个 chrome 插件，根据数据表的 DDL 语句来自动生成 golang 结构体，可以配置 go 结构体字段类型与 mysql 数据表字段类型的一个映射关系。

## SQL2Struct

> SQL2Struct 是一款对 golang 开发者友好的 chrome 插件，根据在 mysql 中创建数据表的 sql 语句，自动生成 golang 中的 struct，在 golang 开发者使用诸如 gorm 之类的框架时，可以很好的把 mysql 中的数据表与 orm 的结构体关联起来。

github 地址：[https://github.com/mikemintang/sql2struct](https://github.com/mikemintang/sql2struct)

## 使用说明

1. 下载源码

```git
git clone https://github.com/mikemintang/sql2struct.git
```

2. 安装扩展

打开扩展中心，选择上一步拉取到的插件源码，本地安装插件。

![](http://blogcdn.idoustudio.com/sql2struct-1.png)

3. 在 mysql 中获取生成数据表的 sql 语句

`show create table users\G;`

![](http://blogcdn.idoustudio.com/Sql.png)

4. 进入插件主页面，把上一步得到的 sql 语句粘贴至左侧的输入框

5. 复制右侧生成的 struct，粘贴至 golang 代码中即可

![](http://blogcdn.idoustudio.com/plugin)

## 配置说明

目前只有三个配置项

- gorm：开启此配置项，则生成 struct 的时候，每个字段都会包含类似`gorm:column:"id"`这样的信息。
- json：开启此配置项，则生成 struct 的时候，每个字段都会包含类似`json:"id"`这样的信息。
- typeMap：此配置项定义 mysql 数据表字段类型与 go 字段类型的映射关系，在数据解析的时候会安装配置的映射关系进行结构体生成。

![](http://blogcdn.idoustudio.com/options)

## Todolist

- [ ] 支持更多的 mysql 类型与 go 类型的映射
- [ ] 支持自定义要进行转换的字段配置
- [ ] 正则表达式优化
- [ ] 数据表名称复数形式与 struct 名称单数形式转换
- [ ] 增加主键、索引转换支持

## Contribution

欢迎 fork 代码、提 issue 或者是 pull request
