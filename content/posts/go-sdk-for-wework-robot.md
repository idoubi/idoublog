---
title: "我写了一款企业微信机器人SDK"
date: 2020-12-02T10:51:34+08:00
draft: false
---

## 创建机器人

在企业微信选择一个群聊，右键点击添加机器人，即可创建一个具备基本消息推送能力的机器人。

![](http://blogcdn.idoustudio.com/pic/webot1.png)

## 下载SDK


```shell
go get -u github.com/cutesdk/webot
```

## 机器人推送消息

使用机器人推送消息非常简单，只需要在创建完机器人之后，拿到机器人的 Webhook地址，使用 SDK 创建一个 Bot 对象，通过链式调用完成消息推送。

### 基本使用

```go
package main

import (
	"github.com/cutesdk/webot"
)

func main() {
	bot := &webot.Bot{
		WebhookURL: "https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key=b8f1e424-d48d-46cc-a2c7-d360c8e98b3d",
	}

	bot.Text("你好，我是机器人").Send()
}
```

运行上面的代码，添加了机器人的群就会收到消息：

![](http://blogcdn.idoustudio.com/pic/webot2.png)

### 发送 markdown 类型消息

```go
	mdmsg := `*这是一条Markdown消息*
> 这是引用文本
- 这是列表1
- 这是列表2	
`
	bot.Markdown(mdmsg).Send()
```

运行上面的代码，机器人会发送 markdown 消息：

![](http://blogcdn.idoustudio.com/pic/webot3.png)


### 发送带操作按钮的 markdown 消息

```go
mdmsg := `这是一条带操作按钮的 Markdown 消息
	> 请确认你要提交吗？`

callbackID := "testwebot"
actions := []webot.MsgAction{}
actions = append(actions, webot.MsgAction{
	Text:        "确认",
	Name:        "acts",
	Value:       "confirm",
	ReplaceText: "已确认",
	Type:        "button",
	TextColor:   "2EAB49",
	BorderColor: "2EAB49",
})
actions = append(actions, webot.MsgAction{
	Text:        "取消",
	Name:        "acts",
	Value:       "cancel",
	ReplaceText: "已取消",
	Type:        "button",
})

bot.Actions(callbackID, actions).Markdown(mdmsg).Send()
```

运行上面的代码，机器人发送带有操作按钮的 markdown 消息：

![](http://blogcdn.idoustudio.com/pic/webot4.png)


点击操作按钮，机器人会给回调地址推送事件消息，开发者可以在回调事件里面进行相应的处理，实现复杂的机器人交互逻辑。

### 提醒谁看

```go
msg := "这是一条要提示用户<@adam>的文本消息"
bot.Mention([]string{"@all"}).Text(msg).Send()
```

运行上面代码，机器人会发送消息，并艾特指定的用户：

![](http://blogcdn.idoustudio.com/pic/webot5.png)


在文本中艾特人用 `<@userid>`，艾特全体成员用 `@all`

### 指定用户可见

```go
msg := "这是一条仅指定用户可见的文本消息"
bot.Target([]string{"wrkSFfCgAAIfjQ7pSijsAAx_KpflhDBw"}).Visible([]string{"adam"}).Text(msg).Send()
```

运行上面的代码，机器人会发送消息到群聊，只有指定的群聊中的指定用户可以看到消息：

![](http://blogcdn.idoustudio.com/pic/webot6.png)


`Target` 用于指定一个或多个群聊的 `chatid` ，每个群有唯一的 `chatid` ，在配置了机器人接收消息后，才能获取到群聊的 `chatid`。

## 机器人接收消息

目前公网版本的企业微信机器人并不支持接收消息配置，暂不能使用机器人完成复杂的交互功能。请关注企业微信官网查看更新动态。