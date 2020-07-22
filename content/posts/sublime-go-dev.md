---
title: 「Sublime」配置Go开发环境
tags:
  - SublimeText3
categories:
  - 系列分享
date: 2019-04-27 17:01:52
---


## 安装GoSublime插件

GoSublime是在Sublimeb编辑器中开发Go语言必备的插件之一，提供Go源码编译、格式化、包自动导入等功能。之前可以通过Sublime的软件仓库直接安装，现在的版本已经搜不到这个插件了，我们需要通过源码进行安装。

- 复制插件源码到Sublime包扩展目录

```sh
# 进入sublime插件目录
cd '/Users/mikemin/Library/Application Support/Sublime Text 3/Packages'

# 下载GoSublime
git clone https://github.com/DisposaBoy/GoSublime.git
```

- 安装Margo依赖

```
# 安装margo
go get github.com/slene/margo

# 配置margo
cd GoSublime/src
mkdir margo
cp margo.sh/extension-example/extension-example.go margo/margo.go
```


- GoSublime插件配置
```
{
    "env": {
        "GOPATH": "/data/go",
        "GOROOT": "/usr/local/opt/go/libexec"
    },
    "fmt_enabled": true,
    "fmt_cmd": [
    	"goimports"
    ]
}
```

> 在GoSublime插件中配置包自动导入前，请先确保安装了`goimports`工具。

[goimports工具安装与使用](http://idoustudio.com/2017/11/06/goimports%E5%B7%A5%E5%85%B7%E5%AE%89%E8%A3%85%E4%B8%8E%E4%BD%BF%E7%94%A8/)

- `ctrl/command + s` 保存文件时，编辑器会自动格式化代码并进行包的导入和删除。
- `ctrl/command + b` 可调出控制台，对go代码进行编译、测试或直接运行。




