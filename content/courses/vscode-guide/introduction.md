---
title:
---

### VS Code 介绍

VS Code 是微软开发并维护的一款代码编辑器，几乎支持所有主流编程语言的代码高亮、智能提示。具备诸多优良的特性：

- 轻量级
- 免费开源
- 跨平台
- 丰富的扩展
- 良好的性能

### VS Code 使用

- 下载地址：[https://code.visualstudio.com/](https://code.visualstudio.com/)
- 官方教程：[https://code.visualstudio.com/docs](https://code.visualstudio.com/docs)

### VS Code 安装扩展

下面三种方式都可以进入扩展安装面板：

1. 选择 Code>Preferences>Extensions
2. 使用快捷键 Shift+Command+X（推荐）
3. 使用快捷键 Shift+Command+P 进入命令面板，输入 `Extensions: Install Extensions`

搜索扩展并进行安装：

![](http://blogcdn.idoustudio.com/vscode-extensions.png)

### VS Code 自定义代码片段

选择 Code>Preferences>User Snippets 或使用快捷键 Shift+Command+P 进入命令面板，输入 `Preferences: Configure User Snippets` 进行自定义代码段的管理。

点击 `New Global Snippets file...` 新建自定义代码段：

custom.code-snippets

```json
{
  "Author Info": {
    "scope": "php,go",
    "prefix": "author",
    "body": ["/**", "* $1", "* @author 艾逗笔<http://idoubi.cc>", "*/", "$2"],
    "description": "Author Infomartion"
  }
}
```

上述自定义代码配置保存后，在 PHP 或 Go 文件中输入 author 即可生成自定义的代码段。

### VS Code 自定义快捷键

输入快捷键 Command+K、Command+S，进入快捷键设置界面。搜索关键词寻找指令，点击左侧的修改按钮，绑定自定义的快捷键。

![](http://blogcdn.idoustudio.com/vscode-shortcuts.png)

也可以在 keybindings.json 中直接修改：

```json
// Place your key bindings in this file to overwrite the defaults
[
  {
    "key": "cmd+l",
    "Command": "workbench.action.toggleSidebarVisibility"
  },
  {
    "key": "cmd+b",
    "Command": "-workbench.action.toggleSidebarVisibility"
  }
]
```

### VS Code 自定义配置参数

输入快捷键 `Command+,`，进入配置页面，搜索关键词调整配置参数。

![](http://blogcdn.idoustudio.com/vscode-settings.png)

也可以在 settings.json 中编辑自定义的配置：

```json
{
  "editor.fontSize": 14, // 字体大小
  "editor.formatOnSave": true, // 保存时自动格式化
  "files.autoSave": "onFocusChange", // 失去焦点时自动保存文件
  "breadcrumbs.enabled": true, // 显示文件路径
  "window.zoomLevel": 0, // 窗口缩放比例
  "workbench.statusBar.visible": true, // 隐藏底部状态栏
  "workbench.colorTheme": "An Old Hope Italic" // 编辑器主题
}
```

### VS Code 自定义主题

对于一个对颜值有要求的开发者，我们希望能够根据自己的喜好定制编辑器的颜色主题。VS Code 内置诸多不同风格的编辑器主题，输入快捷键 Command+K、Command+T 弹出主题选择界面，可以选择自带的主题。也可以在扩展中心安装第三方主题后再在这里启用。

![](http://blogcdn.idoustudio.com/vscode-themes.png)
