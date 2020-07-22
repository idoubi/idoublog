---
title: linux下安装python
categories:
  - 开发笔记
tags:
  - python
date: 2017-10-29 10:29:34
---


## python源码下载页

[https://www.python.org/downloads/source/](https://www.python.org/downloads/source/)


## 安装python

```shell
# 下载源码
wget https://www.python.org/ftp/python/3.6.3/Python-3.6.3.tgz

# 解压缩
tar -zxvf Python-3.6.3.tgz

# 进入源码目录
cd Python-3.6.3

# 配置(指定安装到/usr/local/python3目录)
./configure --prefix=/usr/local/python3

# 编译
make

# 安装
make install
```

## 使用python3作为默认版本

```shell
# 进入bin目录
cd /usr/bin

# 查看所有的python命令
ls | grep python

# 将python3作为默认python
mv python python2.20171027
mv python3 python

# 查看当前python版本
python -V
```

## 安装pip

```shell
# 下载pip安装文件
wget -c https://bootstrap.pypa.io/get-pip.py

# 安装pip
python get-pip.py
python2 get-pip.py
```