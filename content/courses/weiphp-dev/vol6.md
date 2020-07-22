---
title: 微信高级客服接口在weiphp插件中的使用
id: 537
categories:
  - 系列分享
date: 2015-04-20 15:25:55
tags:
  - weiphp
  - weiphp插件开发
---

接到一个用户的需求：想更改一下 weiphp 通用表单插件，当用户在微信端填写表单内容并提交表单后，系统自动把该用户填写的内容发送给指定的用户。

评估了一下这个需求，发现做起来其实还是很容易的，主要就是需要用到微信的高级接口：客服接口。具体思路是：创建自定义通用表单时填写一个需要把表单内容发往的微信用户 uid（该 uid 可通过在微信中回复“uid”查看）——>当用户提交表单成功之后调用微信的高级接口把表单的内容发给指定用户。

下面我们开始更改 weiphp 自带的通用表单（Forms）插件：

1、在智能聊天（Chat）的微信交互模型（/Addons/Chat/Model/WeixinAddonModel.class.php）中实现当用户发送“uid”时返回用户 uid 的功能。

![](http://blogcdn.idoustudio.com/2015/04/12.png)

2、在 weiphp 后台模型管理中找到 forms 模型，新增一个字段 uid。

![](http://blogcdn.idoustudio.com/2015/04/22.png)

3、在微信中查看 uid。

![](http://blogcdn.idoustudio.com/2015/04/32.png)

4、在通用表单插件中新建一个表单，并把表单提交后需要通知的用户 uid 填上去。

![](http://blogcdn.idoustudio.com/2015/04/41.png)

![](http://blogcdn.idoustudio.com/2015/04/52.png)

5、新建几个字段。

![](http://blogcdn.idoustudio.com/2015/04/62.png)

6、在 Application/Common/Common/function.php 中新增几个方法，用于使用微信客服接口（客服接口只有认证了的服务号才能使用）。

![](http://blogcdn.idoustudio.com/2015/04/71.png)

7、在通用表单的控制器中改写表单提交的方法，当表单提交成功后调用客服接口函数给后台指定的用户发消息。

![](http://blogcdn.idoustudio.com/2015/04/8.png)

8、我们现在在微信发送“报名”来触发第 4 步中创建的表单。

![](http://blogcdn.idoustudio.com/2015/04/9.png)

9、点击进入填写表单。

![](http://blogcdn.idoustudio.com/2015/04/10.png)

10、填写好表单之后点“确定”提交表单，被指定的用户在微信中就会立刻接到通知。

![](http://blogcdn.idoustudio.com/2015/04/111.png)

好啦，本次任务圆满完成，客户承诺给的 500 元报酬妥妥的就到手了。

weiphp 的通用表单还是很强大的，可以实现很多功能，而微信高级接口的接入自然让此功能如虎添翼。客服接口属于微信认证服务号独有的接口，有了此接口可以实现很多高级的功能，比如订餐系统中用户提交订单后店主接到订单通知、微信运营者与微信用户实时互动之类的。更多实用、好玩的功能还要靠大家一起去探索，本文只起到抛钻引玉的作用~
