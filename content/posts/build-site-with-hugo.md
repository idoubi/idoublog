---
title: "基于「Hugo」搭建个人博客网站"
date: 2020-04-19T22:11:23+08:00
draft: false
categories:
  - 开发笔记
tags:
  - Hugo
  - 博客网站
---

## 前言

搭建个人网站，以前我们一般会选择 `WordPress`，因为其使用范围广（据说全世界 80%的网站都是用它搭建的）、主题丰富、上手简单。

近年来，`markdown` 内容格式渐渐流行，我们更愿意使用 `markdown` 格式来写文章，写完后用静态网站生成工具把文章内容转换成 `html` 格式，再发布到个人网站。生成静态网站的工具中，比较优秀的有 `Jekyll`、`Hexo`、`Hugo` 几个。

简单比较一下动态网站框架 `WordPress` 与静态网站生成框架 `Hexo`、`Hugo` 的区别：

| 项目      | 开发语言 | 优势                                                                    | 不便之处                                                                              |
| --------- | -------- | ----------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| WordPress | php      | 1. 有可视化后台可以编写文章；<br>2. 使用范围广；<br>3. 主题、插件丰富。 | 1. 依赖过多导致加载较慢；<br>2. 需要服务器部署，依赖数据库；<br>3. 数据需要定时备份。 |
| Hexo      | nodejs   | 1. 静态生成，SEO 友好；<br>2. npm 生态有丰富的插件可用于扩展功能。      | 1. 本地编译生成静态文件速度较慢；<br>2. 调试麻烦，经常遇到 js 报错。                  |
| Hugo      | go       | 1. 编译速度最快;<br> 2. 开发主题非常方便。                              | 1. 目前主题数量较少。                                                                 |

综合来看，我还是比较推荐使用 `Hugo` 来搭建个人网站。

## 安装 Hugo

MacOS 系统下，可以使用 `brew` 来进行安装。

```shell
brew install hugo
```

安装完后，可以全局使用 `Hugo` 命令

```shell
hugo version
```

## 创建个人网站

```shell
hugo new site myblog
```

进入创建的 `myblog` 目录，可以看到生成的目录结构长这样：

```shell
.
├── archetypes
│   └── default.md
├── config.toml
├── content
├── data
├── layouts
├── static
└── themes
```

## 选择一个主题

创建完网站之后，我们可以在 `Hugo` 官方的 [主题商店](https://themes.gohugo.io/) 选择一个自己喜欢的主题，并下载下来使用。

这里以 [hugo-notepadium](https://github.com/cntrump/hugo-notepadium) 这个主题为例，进入上一步创建的个人网站，并克隆主题到 `themes` 文件夹：

```shell
git clone https://github.com/cntrump/hugo-notepadium.git themes/hugo-notepadium
```

```shell
.
├── archetypes
│   └── default.md
├── config.toml
├── content
├── data
├── layouts
├── static
└── themes
    └── hugo-notepadium
```

## 修改网站配置

修改站点配置文件 `config.toml` ，填写自己的网站信息和使用的主题名称，也可以根据 [主题说明](https://github.com/cntrump/hugo-notepadium) 里示例的配置信息来自定义网站内容。

```toml
baseURL = "https://example.com"
title = "MyBlog"
theme = "hugo-notepadium"
copyright = "©2020 my name."
```

## 创建文章

接下来我们可以开始写文章了，通过：

```shell
hugo new posts/helloworld.md
```

新建一篇文章。在生成的文件中使用 `markdown` 格式来书写文章内容。

```markdown
---
title: "Helloworld"
date: 2020-04-19T23:56:47+08:00
draft: true
---

## 说明

> HelloWorld

这是内容
```

## 网站预览

执行 `server` 命令，对所有已发布和编辑中的文章进行预览：

```shell
hugo server -D
```

## 发布内容

写完文章，预览没问题后，可以更改文章的草稿状态 `draft: false`，然后编译生成静态网站内容了：

```shell
hugo -t hugo-notepadium
```

可以看到，几乎瞬间完成编译工作。生成的静态内容都在 `public` 目录下面：

```shell
public
├── 404.html
├── categories
├── css
├── index.html
├── index.xml
├── page
├── posts
├── sitemap.xml
└── tags
```

## 部署到线上

最简单的部署方式，只需要把 `public` 目录下的内容上传到 `Github`，并使用 `Github Pages` 创建一个站点，就可以通过：`xxx.github.io` 访问了，还可以绑定自定义域名。

也可以自己写一个 `shell` 脚本，做到每次编译完文章后自动同步 `public` 目录下的内容到 `Github` 或者自己的服务器，来保持线上站点的内容及时更新。

## 总结

通过前面的步骤我们看到通过 `Hugo` 创建静态网站是非常方便的，并且发布前的编译速度也非常快。比起传统的动态网站，可能不足的地方在于没有可视化后台来随时新增或修改 `markdown` 内容，但是实际上也是可以做到的，我们可以选择开发自己的主题，在主题开发过程中，我们可以通过 `getJSON` 来获取 `API` 传递的动态数据，有了这个功能，说不定就可以很好的结合动态网站与静态网站的优势了。以后有时间再讲一下如何开发自定义主题吧。

## 参考

- [Hugo 官方文档](https://gohugo.io/getting-started/quick-start/)
- [Hugo 主题商城](https://themes.gohugo.io/)
- [hugo-notepadium](https://github.com/cntrump/hugo-notepadium)
