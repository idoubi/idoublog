---
title: "使用「Air」实现热加载，助力「Go」项目开发调试"
date: 2020-07-25T12:04:25+08:00
---

Go 是一种静态编译型语言，写完源代码后，需要编译成二进制可执行文件，才能运行程序看到相应的效果。我们在开发 Go 程序的过程中经常需要调试，如果没有热加载功能，这将是非常痛苦的一件事。

[Air](https://github.com/cosmtrek/air) 是 Go 生态中一个可以实现热加载的库，在开发 Go 项目的时候引入 Air，可以很大程度上提升开发调试的效率。

## 安装Air

```go
go get -u github.com/cosmtrek/air
```

安装完成后，我们执行`air -h`可以看到Air的基本用法：

```shell
$ air -h  
Usage of air:
  -c string
        config path
  -d    debug mode
  -v    show version
```

## 热加载配置

在要开发的 Go 项目目录下新建一个 `.air.conf`，写入基本的配置信息。

```vim
# Config file for [Air](https://github.com/cosmtrek/air) in TOML format

# Working directory
# . or absolute path, please note that the directories following must be under root.
root = "."
tmp_dir = "tmp"

[build]
# Just plain old shell command. You could use `make` as well.
cmd = "make build"
# Binary file yields from `cmd`.
bin = "webapi server"
# Customize binary.
full_bin = "APP_ENV=dev APP_USER=air ./webapi server"
# Watch these filename extensions.
include_ext = ["go", "yaml"]
# Ignore these filename extensions or directories.
exclude_dir = ["config", "tmp", "vendor", "tests"]
# Watch these directories if you specified.
include_dir = []
# Exclude files.
exclude_file = []
# This log file places in your tmp_dir.
log = "air.log"
# It's not necessary to trigger build each time file changes if it's too frequent.
delay = 1000 # ms
# Stop running old binary when build errors occur.
stop_on_error = true
# Send Interrupt signal before killing process (windows does not support this feature)
send_interrupt = false
# Delay after sending Interrupt signal
kill_delay = 500 # ms

[log]
# Show log time
time = false

[color]
# Customize each part's color. If no color found, use the raw app log.
main = "magenta"
watcher = "cyan"
build = "yellow"
runner = "green"

[misc]
# Delete tmp directory on exit
clean_on_exit = true
```

例如我们开发的项目名称叫做 `webapi` ，在引入热加载配置之前，我们要调试程序，需要进行的操作为：

```shell
## 编译生成可执行文件
go build -o webapi main.go
## 启动http服务
./webapi server
```

那么我们可以在热加载文件中配置成：

```ini
[build]
cmd = "go build -o webapi main.go"
bin = "webapi server"
full_bin = "APP_ENV=dev APP_USER=air ./webapi server"
include_ext = ["go", "yaml"]
```

## 热加载方式运行

```shell
air -c .air.conf
```

我们在项目根目录下运行上面的命令，项目就启动了，`Air` 会监听项目中的 `.go`、`.yaml` 类型文件，一旦文件有修改，`Air` 会自动编译程序，并重新启动。

有了热加载，我们就可以专注于写业务逻辑了，预览调试起来方便的很。