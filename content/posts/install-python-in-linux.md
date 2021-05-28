---
title: 在 Linux 系统通过源码安装 Python3
categories:
  - 开发笔记
tags:
  - python
date: 2021-05-28 10:29:34
---

1. 打开 python 源码下载页

[https://www.python.org/ftp/python/](https://www.python.org/ftp/python/)

2. 下载源码

```shell
wget https://www.python.org/ftp/python/3.9.5/Python-3.9.5.tgz
```

3. 解压缩

```shell
tar -zxvf Python-3.9.5.tgz
```

4. 安装

```shell
cd Python-3.9.5

./configure --prefix=/usr/local/python3

make && make install
```

5. 设置环境变量

```shell
export PATH=$PATH:/usr/local/python3/bin
```

6. 备份原来版本

```shell
python --version
pip --version

which python
which pip

mv /usr/bin/python /usr/bin/python2.7.5
mv /usr/bin/pip /usr/bin/pip2.7.5
```

7. 使用新版本

```shell
ln -s /usr/local/python3/bin/python3 /usr/bin/python
ln -s /usr/local/python3/bin/pip3 /usr/bin/pip
```

8. 查看新版本

```shell
python --version
pip --version
```
