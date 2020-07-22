---
title: 「VS Code」打造类sublime的高颜值编辑器
date: 2019-07-08 21:43:33
tags:
  - vscode
  - sublime
categories:
  - 系列分享
---

用惯了 sublime 编辑器，最近因为实在忍受不了 sublime 的低性能，而决定转向`vscode`。然而 vscode 自带的主题并不能满足我对编辑器颜值的要求，幸好在网上找到一款类似 sublime 的皮肤，安装上之后有一种用 sublime 写代码的既视感。记录下来，分享给有同样需求的朋友。

![](http://blogcdn.idoustudio.com/vscode-sublime-theme-1.png)

## 安装主题

MacOS 系统按`Command+Shift+P`弹出命令框，输入`Install Extensions`进入扩展安装页面，搜索`Monokai ST3`和`vscode-icons`扩展并安装之。

点击编辑器左下角的设置图标，在`Color Theme`中选择`Monikai ST3`，在`File Icon Theme`中选择`vscode-icons`即可完整主题的设置。

或者也可以在`User Settings`中设置：

```
{
    "editor.fontSize": 16,
    "workbench.statusBar.visible": false,
    "workbench.iconTheme": "vscode-icons",
    "workbench.colorTheme": "Monokai ST3"
}
```

## 更多主题

在这里还有更多好看的主题：[vscode 主题下载](https://marketplace.visualstudio.com/search?target=VSCode&category=Themes&sortBy=Downloads)

## 参考

- [知乎：求 vs code 主题推荐？](https://www.zhihu.com/question/38435139)
- [2018 年最佳 VS Code 主题](https://www.jianshu.com/p/55a67702e134)
- [干货 | 教你打造一款颜值逆天的 VS Code](https://juejin.im/entry/587e0f2f570c352201113e14)
