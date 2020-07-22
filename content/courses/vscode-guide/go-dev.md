### 安装扩展

使用快捷键 Shift+Command+X 打开扩展安装页，安装扩展 Go。

![vscode-go](http://blogcdn.idoustudio.com/vscode-go.png)

### 自定义扩展配置

使用快捷键 `Command+,` 打开自定义配置页，编辑 settings.json，定义与 Go 扩展相关的配置项。

```json
{
  "editor.formatOnSave": true,
  "files.autoSave": "onFocusChange",
  "go.buildOnSave": "workspace",
  "go.lintOnSave": "package",
  "go.vetOnSave": "package",
  "go.buildFlags": [],
  "go.lintFlags": [],
  "go.vetFlags": [],
  "go.coverOnSave": false,
  "go.autocompleteUnimportedPackages": true,
  "go.useLanguageServer": true,
  "go.inferGopath": true,
  "go.docsTool": "godoc",
  "go.gocodePackageLookupMode": "go",
  "go.gotoSymbol.includeImports": true,
  "go.useCodeSnippetsOnFunctionSuggest": true,
  "go.useCodeSnippetsOnFunctionSuggestWithoutType": true,
  "go.formatTool": "goreturns",
  "go.gocodeAutoBuild": false,
  "go.liveErrors": {
    "enabled": true,
    "delay": 0
  },
  "go.gopath": "/data/go",
  "go.goroot": "/usr/local/Cellar/go/1.12.7/libexec"
}
```

### 安装依赖

![](http://blogcdn.idoustudio.com/vscode-go-install.png)

第一次编辑完 Go 代码保存的时候，VS Code 会提示需要安装依赖，点击 `Install All` 进行安装。如果遇到墙的问题，则需要手动安装依赖，需要先下载依赖源码，再进行安装。

```shell
go get -u -v github.com/ramya-rao-a/go-outline
go get -u -v github.com/acroca/go-symbols
go get -u -v github.com/mdempsky/gocode
go get -u -v github.com/rogpeppe/godef
go get -u -v golang.org/x/tools/cmd/godoc
go get -u -v github.com/zmb3/gogetdoc
go get -u -v golang.org/x/lint/golint
go get -u -v github.com/fatih/gomodifytags
go get -u -v golang.org/x/tools/cmd/gorename
go get -u -v sourcegraph.com/sqs/goreturns
go get -u -v golang.org/x/tools/cmd/goimports
go get -u -v github.com/cweill/gotests/...
go get -u -v golang.org/x/tools/cmd/guru
go get -u -v github.com/josharian/impl
go get -u -v github.com/haya14busa/goplay/cmd/goplay
go get -u -v github.com/uudashr/gopkgs/cmd/gopkgs
go get -u -v github.com/davidrjenni/reftools/cmd/fillstruct
go get -u -v github.com/alecthomas/gometalinter
gometalinter --install
```

部分依赖源码地址：

- [golang.org/x/tools](https://github.com/golang/tools)
- [golang.org/x/lint](https://github.com/golang/lint)
- [golang.org/x/xerrors](https://github.com/golang/xerrors)

### 使用扩展命令

安装完 Go 扩展后，有一些在扩展中自定义的命令可以使用。

快捷键 Shift+Command+P 打开命令面板，输入 `Go:` 查看所有可以使用的扩展命令。

几个常用的命令：

- `Go: Current GOPATH` 查看当前 GOPATH 的路径
- `Go: Show All Commands` 查看所有可用的扩展命令
- `Go: Install/Update Tools` 安装/更新依赖的工具
- `Go: Browse Packages` 浏览包

### 断点调试

在项目根目录下创建 .vscode/lauch.json 并配置调试参数：

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "gotest",
      "type": "go",
      "request": "launch",
      "mode": "debug",
      "remotePath": "",
      "port": 10550,
      "host": "127.0.0.1",
      "program": "/data/go/src/test/main.go",
      "env": {
        "GOPATH": "/data/go"
      },
      "args": [],
      "showLog": true
    }
  ]
}
```

在项目文件中打断点，按 F5 开始进行断点调试。

如果遇到报错：

> could not launch process: executables built by Go 1.11 or later need Delve built by Go 1.11 or later

升级安装 [delve](https://github.com/go-delve/delve)

```
go get -u github.com/go-delve/delve/cmd/dlv
```

![](http://blogcdn.idoustudio.com/vscode-go-dev.png)
