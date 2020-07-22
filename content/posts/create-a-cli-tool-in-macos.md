---
title: 使用「Go」语言开发命令行工具
categories:
  - 开发笔记
tags:
  - go
  - brew
date: 2020-01-04 16:50:37
---

使用 MacOS 做开发的朋友都知道，我们一般会使用 Homebrew 做软件包管理，经常会用到 `brew install [soft]` 来安装各种各样的命令行软件。今天通过一个百科查找的命令行工具（`tellme`）示例，我们来学习一下如何使用 Go 语言开发自己的命令行软件。

我们需要用到 [cobra](https://github.com/spf13/cobra) 这个 Go 模块来做命令行工具开发，这个开源库其实是对 Go 官方库 [flag](https://golang.org/pkg/flag/) 的一个封装，可以简化获取参数的操作。

## 创建命令行项目

- 开启 Go Module

```
export GO111MODULE=on
```

- 安装 cobra 工具

```shell
go get -u github.com/spf13/cobra/cobra
```

- 创建命令行项目

```shell
# 创建项目目录
mkdir -p /data/idoubi/tellme && cd /data/idoubi/tellme

# 定义模块
go mod init github.com/idoubi/tellme

# 初始化命令行项目
cobra init --pkg-name github.com/idoubi/tellme
```

- 检测运行

```shell
go run main.go -h
```

执行完上述操作后，如果控制台输出了帮助信息，证明我们的命令行项目创建成功了。

## 新建子命令

- 新建子命令

```shell
cobra add baike
```

- 编写业务逻辑

在生成的子命令文件 `/data/idoubi/tellme/cmd/baike.go` 中编写子命令需要实现的业务逻辑。

```go
package cmd

import (
	"fmt"
	"os"
	"os/exec"
	"runtime"

	"github.com/spf13/cobra"
)

var (
	platform string
)

var openCmds = map[string]string{
	"windows": "cmd /c start",
	"darwin":  "open",
	"linux":   "xdg-open",
}

var baikeCmd = &cobra.Command{
	Use:     "baike",
	Aliases: []string{"bk", "wk", "wiki"},
	Short:   "find things in baike site",
	Args:    cobra.ExactArgs(1),
	Run: func(cmd *cobra.Command, args []string) {
		err := findInBaike(args[0], platform)
		if err != nil {
			fmt.Println(err)
			os.Exit(1)
		}
	},
}

func init() {
	rootCmd.AddCommand(baikeCmd)
	baikeCmd.Flags().StringVarP(&platform, "platform", "p", "baidu", "platform to find things")
}

// 百科查找
func findInBaike(keyword, platform string) error {
	var link string
	// 百度百科搜索
	if platform == "baidu" || platform == "bd" {
		link = fmt.Sprintf("https://baike.baidu.com/item/%s", keyword)
	}
	// 互动百科搜索
	if platform == "hudong" || platform == "baike" || platform == "hd" {
		link = fmt.Sprintf("http://www.baike.com/wiki/%s", keyword)
	}
	// 维基百科搜索
	if platform == "wikipedia" || platform == "wiki" || platform == "wp" {
		link = fmt.Sprintf("https://zh.wikipedia.org/wiki/%s", keyword)
	}
	if link == "" {
		return fmt.Errorf("invalid platform")
	}
	goos := runtime.GOOS
	opencmd := "open"
	opencmd, ok := openCmds[goos]
	if !ok {
		return fmt.Errorf("can not open link in %s", goos)
	}
	if err := exec.Command(opencmd, link).Start(); err != nil {
		return err
	}

	return nil
}
```

## 命令测试

- 项目中运行

在命令行项目目录下，通过 `go run` 可以直接运行，查看输出结果。

```shell
# 在百度百科查看信息
go run main.go baike 周杰伦

# 在维基百科查看信息
go run main.go bk -p wp 周杰伦
```

上述的 `baike` 是我们创建的子命令，`bk` 是子命令别名，`-p` 是子命令标识，用于指定百科平台。`周杰伦` 是接收的参数。

- 编译运行

在命令行项目目录下，通过 `go build` 可以将项目编译成一个二进制文件，通过 `./tellme` 可以直接运行二进制文件查看输出。

```shell
# 编译
go build -o tellme

# 运行
./tellme -h
```

- 交叉编译

除了在 MacOS 中使用外，我们还希望能在 linux 和 windows 系统中使用我们的命令行工具。这种情况下我们需要用到交叉编译。

```shell
# 下载交叉编译工具
go get -u github.com/mitchellh/gox

# 编译成指定平台的可执行文件
gox -osarch "windows/amd64 linux/amd64 darwin/amd64"
```

- 全局执行

编译完成后，我们可以创建一个软连接，让命令行工具可以在任何路径下全局执行。

```shell
# 创建软连接
ln -s /data/idoubi/tellme/tellme_darwin_amd64 /usr/local/bin/tm

# 全局执行
tm -h
```

## 上传到 Github

经过上述的几个步骤，我们已经完成了一个简单的命令行工具的编写与测试工作，接下来需要把软件发布出去让所有人使用。

```shell
# 推送代码到github
cd /data/idoubi/tellme
go init
git add .
git commit -m "a cli tool used in macos"
git push origin master

# 发布一个版本
git tag -a v0.1.0 -m "first version"
git push origin --tags
```

上传到 Github 完成后，我们可以在项目的 release 页面看到这个项目的版本信息和源码下载文件 [https://github.com/idoubi/tellme/releases/tag/v0.1.0](https://github.com/idoubi/tellme/releases/tag/v0.1.0)，我们可以编辑发布信息，上传交叉编译后得到的各个平台的二进制文件，方便不同平台的用户在这里直接下载二进制文件来使用我们的软件。

## 创建 Homebrew 软件

对于 MacOS 用户，一般都是使用 Homebrew 来安装和管理各种软件，我们希望把自己开发的命令行软件发布到 Homebrew，让用户可以方便的使用 `brew install tellme` 来安装。

- 创建 Homebrew 软件

我们可以在 Github 上复制软件源码包下载地址，在 MacOS 通过 `brew create [source]` 创建 Homebrew 软件包。

```shell
brew create https://github.com/idoubi/tellme/archive/v0.1.0.tar.gz
```

执行上述命令完成后，会在 `/usr/local/Homebrew/Library/Taps/homebrew/homebrew-core/Formula/` 目录生成一个 `tellme.rb` 文件，即我们的 Homebrew 软件定义文件，在此文件中定义了我们的软件在 MacOS 中要怎么安装。

- 编辑软件定义文件

因为我们这个命令行软件是用 Go 语言编写的，在 Homebrew 软件定义文件中，我们可以指定软件的安装依赖于 `go:build`，并且定义好 `install` 方式。

```ruby
class Tellme < Formula
  desc "a cli tool to get information."
  homepage "https://github.com/idoubi/tellme"
  url "https://github.com/idoubi/tellme/archive/v0.1.0.tar.gz"
  sha256 "b0c14c0e9f02e065917262a7c0d16e205097cc4d10c01e03ad93b4cd737cf81d"

  depends_on "go" => :build

  def install
    system "go", "build", "-o", bin/"tellme"
  end

  test do
    system "false"
  end
end
```

- 通过可执行文件创建 Homebrew 软件

上面看到的是通过 Go 源码编译的方式定义 Homebrew 软件的安装方式，我们还可以通过下载可执行二进制的方式定义 Homebrew 软件包的安装，比如通过交叉编译生成可执行文件。

基于可执行文件创建 Homebrew 软件包：

```shell
brew create https://github.com/idoubi/tellme/releases/download/v0.1.0/tellme_darwin_amd64.tar.gz
```

编辑 Homebrew 软件包的定义文件 `tellme.rb`:

```ruby
class Tellme < Formula
  desc "a cli tool to get information"
  homepage ""
  url "https://github.com/idoubi/tellme/releases/download/v0.1.0/tellme_darwin_amd64.tar.gz"
  sha256 "164787b02050faef0d5776ca86d27d15b26fe28493f95ba2e705ba2e30026c94"

  def install
    bin.install "tellme_darwin_amd64" => "tellme"
  end

  test do
    system "false"
  end
end
```

- 安装 Homebrew 软件

创建完 Homebrew 软件后，我们通过 `brew install tellme` 就可以基于本地生成的`/usr/local/Homebrew/Library/Taps/homebrew/homebrew-core/Formula/tellme.rb` 文件来安装我们的命令行软件了。

如果安装报错了，可以通过 `brew edit tellme` 继续编辑 Homebrew 软件定义文件。

## 发布 Homebrew 软件

经过上面的步骤，我们创建了一个在 MacOS 通过 Homebrew 安装的软件，接下来我们需要把这个软件发布出去，让用户能够通过简单的 `brew` 命令直接安装使用。

- 发布到 Github 仓库

```
# 创建软件仓库
mkdir -p /data/idoubi/homebrew-tools && cd /data/idoubi/homebrew-tools

# 复制软件定义文件到仓库目录
cp /usr/local/Homebrew/Library/Taps/homebrew/homebrew-core/Formula/tellme.rb /data/idoubi/homebrew-tools/

# 推送到 Github 仓库
git init
git remote add origin https://github.com/idoubi/homebrew-tools.git
git add .
git commit -m "a homebrew soft named tellme"
git push origin master
```

- 安装软件

软件定义文件推送到 Github 仓库后，可以删除本地的 `/usr/local/Homebrew/Library/Taps/homebrew/homebrew-core/Formula/tellme.rb` 文件。

任何人可以通过下面两行命令在自己的 MacOS 系统中使用我们开发的命令行软件。

```shell
# 连接软件仓库
brew tap idoubi/tools
# 从软件仓库安装软件
brew install tellme
```

## 总结

前面的内容简单介绍了如何使用 Go 语言来创建一个在 MacOS 使用的命令行软件，需要开发者熟悉 MacOS 中 Homebrew 软件包管理工具的使用以及 Go 语言。后面我们可以展开更多的想象，在我们的命令行工具中添加查天气、搜歌词、翻译、时间类型转换等各种特性功能。

- 源码

命令行软件源码：[tellme](https://github.com/idoubi/tellme)

Homebrew 仓库：[homebrew-tools](https://github.com/idoubi/homebrew-tools)

## 参考

- [Homebrew 官网](https://brew.sh/)
- [Go 官方库 flag](https://golang.org/pkg/flag/)
- [Go 命令行软件创建工具 cobra](https://github.com/spf13/cobra)
- [使用 Homebrew 维护自己的软件仓库](https://mogeko.me/2019/046/)
- [在 Homebrew 上发布自己的 App](https://liam.page/2016/07/30/release-your-own-app-in-Homebrew/)
