---
title: goimports工具安装与使用
categories:
  - 开发笔记
tags:
  - go
  - goimports
date: 2017-11-06 10:40:16
---


## goimports是干嘛的

go是一门强类型的编译型语言，有着很严格的类型约束与语法规范，在golang代码中，如果使用到的包没有被引入或者是引入了的包没有被使用，都会编译不通过。所以我们在写go代码的时候，用到包的时候需要首先import一下，不用的时候，要把import包的语句删除或者是注释，但是总是这样手动去操作并不是很方便，比如我们在开发的时候需要用`fmt`包打印一些调试信息，为了让打印出来的数组或结构体格式好看一点，我们有时候需要使用`encoding/json`包来进行格式化。在调试完成后，我们必须要手动把引入的包删除掉，然后再进行编译。

针对上面提到的日常开发中经常面对的手动管理包的问题，`goimports`出现了。`goimports`可以自动对代码中的依赖包进行管理，如果有用到，就会自动import，也会对没有用到的包进行自动删除。

## 安装goimports

因为被墙的原因，直接通过`go get`可能不能正确拉取到`goimports`包，我们这里使用`github`源码编译的方式进行安装。

1. 拉取github上面的tools包源码

```git
git clone https://github.com/golang/tools.git /d/go/src/golang.org/x/tools
```

2. 进入goimports命令包目录

```shell
cd /d/go/src/golang.org/x/tools/cmd/goimports
```

3. 编译源码

```go
go build
```

编译完成后，在windows下会生成goimports.exe可执行文件，在mac跟linux下的goroot配置的bin目录下会生成goimports可执行文件。如果设置了环境变量，在任意位置可执行goimports命令。

## 在sublime配置goimports

在sublime安装GoSublime插件，在插件用户配置文件中写入：

```json
{
	"fmt_cmd": ["goimports"]
}
```

配置完成后在sublime编写go代码时，每次`ctrl+s`保存文件或者`ctrl+b`编译文件时，goimports会自动执行并对go代码进行包依赖的检查，对于用到却未引入的包会进行自动引入，对于引入却未使用的包会进行自动删除。

## 在gogland中使用goimports

gogland原生支持goimports，不需要进行额外的配置，保存或编译go代码时会自动进行包的依赖检查。