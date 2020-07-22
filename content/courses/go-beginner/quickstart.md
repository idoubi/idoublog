## MacOS 安装 Go

在 `MacOS` 安装 `Go` 非常简单，只需要使用

## VS Code 配置 Go 开发环境

在 `VS Code` 编辑器中进入 `Code > Preferences > Extensions`，搜索 `Go` 插件并安装。

## 第一个 Go 程序

在 `main.go` 中写入下面的代码：

```go
package main

func main() {
	println("hello world!")
}
```

在终端执行 `go run main.go`，将会输出：

```shell
$ go run main.go
hello world!
```
