---
title: 在个人博客实现「小课」系统
date: 2020-04-15T22:58:04+08:00
categories:
  - 开发笔记
tags:
  - 小课
  - hugo
draft: false
---

程序员这个行业，是一个终身学习的行业，经常会有新技术出现。我是一个很爱学习的人，喜欢尝鲜。每次想去学习一项新技术的时候，总是苦于找不到合适的入门教程。于是有了想自己写个小课系统的想法：希望能通过写一系列的文章，组织成一门小课，帮助想学习某项技术的同学快速入门。

有了这个想法之后，我开始准备搭建小课平台，一开始想的是做一个前后端分离的网站，后端用 Go 写 API，前端用 vue 写页面。

使用 `vue` + `ElementUI` 搭建出来的第一个版本长这样：

![](http://blogcdn.idoustudio.com/pic/20200415232811.png)

![](http://blogcdn.idoustudio.com/pic/20200415233104.png)

![](http://blogcdn.idoustudio.com/pic/20200415233304.png)

写完第一个版本之后，仔细想了一下，如果要写好这个小课系统的话，我还得做好几件事：

1. 写 API ，前后端进行数据交互
2. 写 Markdown 编辑器，在线录入课程
3. 做服务端渲染，做 SEO 优化

一想到这里，头开始有点大，第一个对外发布的小课系统，我希望能尽可能简洁，不管是写作，还是展示，都不要太复杂。而且最重要的一点，我希望能有更多人看到我的小课系统，也就是说 SEO 一定要好。

考虑到这些因素，我决定用静态博客生成系统 `Hugo` ，基于我的个人博客来完成小课系统的开发。

使用 `Hugo` 来开发，有这么几个好处：

1. 不需要写 Markdown 编辑器，基于本地的 md 文件即可生成文章
2. 不需要使用数据库，课程数据都是 md 文件，维护成本低
3. 不需要做 SSR 也能有很好的 SEO

但是也有几个不好的地方：

1. 纯静态网站，没有数据交互，要做付费订阅等功能不太好实现
2. 每次写完文章，需要编译生成静态页面，再提交发布，比较麻烦

管他三七二十一，先撸一个版本出来再说。于是我就这么干了：

使用 `Hugo` 搭建了个人博客，采用了 `notepadium` 这个开源主题。在 `Hugo` 的 content 里面创建了一个 `courses` 的 section，并为这一类型的 section 新增了一个首页模板和阅读模板。

最终写成的效果：

![](http://blogcdn.idoustudio.com/pic/20200415234021.png)

![](http://blogcdn.idoustudio.com/pic/20200415234141.png)

写完发布上线，我把我之前写的两门小课都放上来了

- [「VS Code」快速上手指南](https://idoubi.cc/courses/vscode-guide/introduction/)
- [分分钟上手「WeiPHP」插件开发](https://idoubi.cc/courses/weiphp-dev/vol1/)

总结一下，我觉得 `Hugo` 是一个非常优秀的静态网站生成工具，功能非常强大，而且编译速度非常快，甩同类型的 `Hexo` 几条街。

后面有时间，我打算写一个讲如何用 `Hugo` 搭建静态网站以及开发自定义主题的小课，希望能有更多的人用上这个用 `Go` 写的，性能卓越的静态网站生成工具。

[Hugo 快速入门官方文档](https://gohugo.io/getting-started/quick-start/)

> Last But Not Least

小课系统第一版写完了，我以后要坚持写小课了。希望能把我已经会的，即将要学的知识都分享出来，让更多的人享受学习的乐趣。

欢迎关注我的公众号，或在评论区给我留言。

<img style="width:300px;height:340px;margin-top:10px;" src="http://blogcdn.idoustudio.com/idoubi-mp.jpeg">
