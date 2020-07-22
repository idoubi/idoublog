---
title: 「Hexo」设置代码语法高亮
tags:
  - hexo
categories:
  - 系列分享
date: 2019-04-27 17:01:16
---

> 在使用某个hexo主题的时候，我们有时候会觉得文章详情中的代码高亮显示的样式并不是很好看，通过下面的方式我们可以选择一套自己喜爱的代码高亮样式，让我们的文章阅读起来更有视觉触动。

## 配置markdown渲染引擎

- 使用新的渲染引擎

```
npm un hexo-renderer-marked --save
npm i hexo-renderer-markdown-it --save
```

- 在_config.yml中配置渲染参数

```
markdown: 'commonmark'
```

- 重新预览

```
hexo clean
hexo s --draft
```

## 配置代码高亮

- 设置code-prettify

```
# 下载源码
git clone https://github.com/google/code-prettify.git

# 拷贝样式
cp code-prettify/src/prettify.css hexo/themes/hyde/source/css/
cp code-prettify/src/prettify.js hexo/themes/hyde/source/js/

```

- 设置皮肤

```
# 下载源码
git clone https://github.com/jmblog/color-themes-for-google-code-prettify.git

# 拷贝样式
cd color-themes-for-google-code-prettify/dist/themes
cp tomorrow-night.min.css hexo/themes/hyde/source/css/
```

- 禁用默认的高亮设置

```
# 修改_config.yml
highlight:
  enable: false
  line_number: false
  auto_detect: false
  tab_replace: false
```

- 在主题模板文件中设置高亮
```
# head标签里引入css
<%- css('css/prettify.css', 'css/tomorrow-night.min') %>

# 文档底部引入js
<script src="https://cdn.bootcss.com/jquery/3.4.0/jquery.min.js"></script>
<%- js('js/prettify.js') %>

# 高亮设置
<script type="text/javascript">
$(window).on('load', function(){ 
  $('pre').addClass('prettyprint linenums');
  prettyPrint();
});
</script>
```

- 刷新预览

```
hexo clean
hexo s --draft
```

## 参考

- [Hexo博客(12)使用google-code-prettify代码高亮](http://masikkk.com/article/hexo-12-google-code-prettify/)
- [hexo-renderer-markdown-it 插件 详解](https://www.jianshu.com/p/588ab3d22eb8)
- [google-code-prettify](https://github.com/google/code-prettify)
- [google-code-prettify-themes](https://jmblog.github.io/color-themes-for-google-code-prettify/)