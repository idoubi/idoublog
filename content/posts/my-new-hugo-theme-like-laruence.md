---
title: "我写了一款跟PHP鸟哥一样的博客皮肤"
date: 2020-11-22T12:23:45+08:00
---

我从2013年开始搭建自己的博客网站，最初用的 `WordPress`，买了一台云主机，下了一个 CMS 风格的皮肤，当初的网站长这样：

![](https://imgkr2.cn-bj.ufileos.com/976cebbc-ee6f-4251-97e6-4d38ce0aaa03.png?UCloudPublicKey=TOKEN_8d8b72be-579a-4e83-bfd0-5f6ce1546f13&Signature=s4kwrACjgq4P4gJ0NvIT4j1KIvY%253D&Expires=1606100749)

后来我学习了 `Markdown` 语法，接触了一些静态网站生成工具，就折腾着把原来的网站迁移到了 `Hexo`。自己动手写了一个皮肤，整成了这个样子：

![](https://imgkr2.cn-bj.ufileos.com/3133c5d2-a9d8-420b-8299-e4eec6407268.png?UCloudPublicKey=TOKEN_8d8b72be-579a-4e83-bfd0-5f6ce1546f13&Signature=5H6zZwZMWqf8Hjh682dGx4VnPts%253D&Expires=1606101765)


后来慢慢的发现，使用 Hexo ，每次构建发布的时候太慢了，有时候换个皮肤，会遇到一大堆 js 报错，自己开发皮肤的时候也不太好调试。

然后接触到了 `Hugo`，Go 语言实现的静态站点生成器，号称“世界上最快的静态网站生成框架”，无论是内容生成速度还是开发效率都非常的高。

果断从 Hexo 换到了 Hugo。几乎没有什么迁移成本，把 markdown 文章拷贝一下，就直接跑起来了。

网站生成工具换了之后，接下来最重要的事情，就是要整一个好看的皮肤。
我上一个使用的皮肤长这样：

![](https://imgkr2.cn-bj.ufileos.com/399d53a7-2368-4edb-8898-37d8fca2b27f.png?UCloudPublicKey=TOKEN_8d8b72be-579a-4e83-bfd0-5f6ce1546f13&Signature=QLHuyF7oKXSfsQRHOCx9QROmcjY%253D&Expires=1606102893)

每隔一段时间，我都会觉得在用的博客皮肤不好看，然后就总想着换一个皮肤，在网上找一圈没有喜欢的就会自己写。

前阵子看到了 PHP鸟哥 的个人网站：

![](https://imgkr2.cn-bj.ufileos.com/26abd7ae-e5cc-4a33-af74-6ec34f4a03ec.png?UCloudPublicKey=TOKEN_8d8b72be-579a-4e83-bfd0-5f6ce1546f13&Signature=TbGPRjlkdmczB1CCyyAUoG6bvTE%253D&Expires=1606105818)

当时就被吸引了，我就喜欢这种简洁大气黑白配的风格。于是便有了把鸟哥的博客皮肤copy下来的想法。

还好 Hugo 的主题开发工作比较简单，官网文档很详细，差不多花了两个小时就把基本的页面写完了，又花了一个礼拜的时间做了一些优化。

最终写完了这个 Hugo 皮肤：[hugo-theme-period](https://github.com/idoubi/hugo-theme-period)。

为什么要叫 `period`，因为我发现鸟哥的网站用的 WordPress 的主题名字也叫 period，我做的是 `wordpress-theme-copy-to-hugo` 的工作，所以主题名称保持一致吧。

现在这款皮肤已经在我的个人网站：[idoubi.cc](https://idoubi.cc) 用上了，预览效果是这样：

![](https://imgkr2.cn-bj.ufileos.com/51b819c3-4314-411b-81da-2dc0a95824e0.png?UCloudPublicKey=TOKEN_8d8b72be-579a-4e83-bfd0-5f6ce1546f13&Signature=tzp4nTeN%252Fi%252BIU42zkHIjjQoQBq8%253D&Expires=1606103516)

我在这款主题支持了几个右侧的挂件，分别是

- [widget.about] 作者简介
- [widget.projects] 开源项目展示
- [widget.qrcode] 公众号二维码
- [widget.categories] 文章分类展示
- [widget.tags] 文章标签展示
- [widget.links] 友情链接

如果你下载了我的这个主题，你可以根据自己的需求，配置对应的挂件。

这款皮肤已经在 `Github` 开源，地址在：[https://github.com/idoubi/hugo-theme-period](https://github.com/idoubi/hugo-theme-period)

我也把这款皮肤提交到了 Hugo 的官方主题仓库，等审核通过后就能在 [Hugo官方主题商店](https://themes.gohugo.io/) 看到。

欢迎大家下载使用，给个Star。

## More

还是要强烈推荐一波 `Hugo`，这真的是我用过的最好的一个开发工具。除了写静态网站，你还可以用它来写项目文档、API等，扩展性很强，文档非常友好。

之前写过一篇文章：[基于「Hugo」搭建个人博客网站](https://idoubi.cc/posts/build-site-with-hugo/)，对 Hugo 的基本使用做了简单的科普。后面有时间我打算写一写 Hugo 开发自定义皮肤的一些关键技巧，希望能有更多的人关注 Hugo、使用 Hugo。

欢迎关注我的公众号获取更多信息。

![](https://blogcdn.idoustudio.com/idoubi-mp.jpeg)



