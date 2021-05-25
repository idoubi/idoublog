---
title: "在 Linux 系统通过源码安装 Go"
date: 2021-05-25T11:19:43+08:00
draft: false
---

1. 安装旧版本的 Go

Go 语言是自举的，要安装新版的 Go 语言，需要依赖系统已有的 Go 语言环境。

在 Centos 中，我们可以通过 yum 来安装 Go 。

```shell
yum install -y go
```

安装完之后查看 Go 版本

```shell
go version
```

可以看到已安装的 Go 版本为： `go version go1.15.5 linux/amd64`

2. 通过源码安装/更新最新版本的 Go

打开 Github 上的 go 发布页

[https://github.com/golang/go/releases](https://github.com/golang/go/releases)

复制发布包链接，在 Centos 下载源码文件

```shell
wget https://github.com/golang/go/archive/refs/tags/go1.16.4.zip
```

解压缩，并安装

```shell
unzip go1.16.4.zip
mv go1.16.4 /usr/local/go1.16.4

export GOROOT=/usr/local/go1.16.4
cd $GOROOT/src
./all.bash
```

3. 修改环境变量

```vim
export GOROOT="/usr/local/go1.16.4"
export GOPATH="/data/code/go"
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
```

4. 替换默认的 Go 版本

```shell
which go
mv /usr/bin/go /usr/bin/go1.15.5
```

5. 查看最新的 Go 版本

```shell
go version
```

可以看到最新安装的 Go 版本为： `go version go1.16.4 linux/amd64`
