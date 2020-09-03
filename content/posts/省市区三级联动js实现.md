---
title: 省市区三级联动js实现
id: 594
categories:
  - 开发笔记
date: 2015-11-03 12:53:27
tags:
  - 三级联动
  - javascript
---

接了一个广州电信微信公众号开发的单子，用户在兑换礼品的时候需要填写个人信息，这个时候需要用到省市区三级联动的功能。![](http://blogcdn.idoustudio.com/2015/11/dx.png)

于是开始在网上找省市区三级联动的 js，终于找到一篇：[http://www.cnblogs.com/zjfree/archive/2013/08/20/3269864.html](http://www.cnblogs.com/zjfree/archive/2013/08/20/3269864.html)

这是使用 QQ 网站用的 js 来实现省市区三级联动的，既然腾讯都用了，姑且我也用这个 js 吧。

下面讲一下使用过程：

## 复制一份生成省市区联动数据的 js：

[http://ip.qq.com/js/geo.js](http://ip.qq.com/js/geo.js)

## 将 js 文件上传到服务器项目目录下面：

![](http://blogcdn.idoustudio.com/2015/11/33.png)

## 在需要用到省市区联动的 html 页面引入该 js：

```
<script type="text/javascript" src="{:ADDON_PUBLIC_PATH}/js/geo.js"></script>
```

## 给省市区联动的 select 分别添加 id：s1、s2、s3

```
<select name="province" id="s1" disabled="disabled">
<option></option>
</select>
<select name="city" id="s2">
<option></option>
</select>
<select name="area" id="s3">
<option></option>
</select>
```

## 给 body 标签绑定 onload 事件用于初始化省市区

```
<body onload="setup();preselect('陕西省');promptinfo();">
```

ok，这样就妥了。省市区三级联动完美实现。
