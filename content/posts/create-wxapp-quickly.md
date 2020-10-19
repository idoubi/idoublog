---
title: "快速创建微信小程序"
date: 2020-10-19T14:27:56+08:00
---

## 前言

做过微信小程序开发的朋友应该知道，微信提供了几种创建小程序的方式：

|                | 个人类型                                       | 机构类型                                                           |
| -------------- | ---------------------------------------------- | ------------------------------------------------------------------ |
| 微信官网注册   | 填写个人身份证、手机号等信息，扫码绑定个人微信 | 填写机构代码、法人手机号等信息，上传营业执照，需要法人微信扫码验证 |
| 公众号后台注册 | 不支持                                         | 复用已认证的微信服务号资质创建小程序，最多创建五个                 |
| 第三方平台注册 | 不支持                                         | 利用微信第三方平台资质，快速创建小程序                             |

对比一下这几种方式，我们发现：

如果要在微信官网申请小程序，需要填很多资料，并且需要为新注册的小程序绑定一个身份证、手机号、微信，很容易受到个人可创建小程序数量的限制。

如果复用公众号资质创建小程序，需要先有一个已通过微信认证的服务号，申请公众号认证的审核费用单次 300 元，每个公众号最多只能创建五个小程序。

如果用第三方平台创建小程序，需要有企业资质，在填写完企业的基本信息后发起创建小程序申请，企业法人的微信会收到创建小程序的通知，根据指引完成法人身份认证之后即可完成小程序的创建，绑定一个新的邮箱即可登录小程序管理后台。使用这种方式，目前来看没有小程序数量和创建人身份的限制。[第三方平台快速创建小程序官方文档](https://developers.weixin.qq.com/doc/oplatform/Third-party_Platforms/Mini_Programs/Fast_Registration_Interface_document.html)

在了解完这些规则之后，我们应该想到，如果有企业资质，创建小程序最好的办法就是利用第三方平台，基本上能够满足一个企业日常的小程序数量需求。

## 快速创建小程序功能体验

微信扫码进入小程序“极速申请”

![qrcode](https://blogcdn.idoustudio.com/wxareg/qrcode.jpg)

微信授权完成登录

![login](https://blogcdn.idoustudio.com/wxareg/login.png?imageView2/1/w/300/h/400)

点击“新建申请”进入企业资料填写页

填写完企业资料，点击“申请注册小程序”

![register](https://blogcdn.idoustudio.com/wxareg/register1.png?imageView2/1/w/300/h/500)

法人微信收到创建小程序的通知

![notify](https://blogcdn.idoustudio.com/wxareg/notify1.png?imageView2/1/w/300/h/350)

完成法人身份验证

![verify](https://blogcdn.idoustudio.com/wxareg/verify.png?imageView2/1/w/300/h/420)

完成小程序创建申请

![finished](https://blogcdn.idoustudio.com/wxareg/finished.png?imageView2/1/w/300/h/350)

收到小程序创建成功通知

![oknotify](https://blogcdn.idoustudio.com/wxareg/oknotify.png?imageView2/1/w/300/h/600)

为新创建的小程序设置登录邮箱

![setemail](https://blogcdn.idoustudio.com/wxareg/setemail.png?imageView2/1/w/300/h/500)

至此，已经完成了一个小程序的创建，非常的快速方便。

再次进入”极速申请“小程序，可以选择已有的企业快速创建小程序
![setemail](https://blogcdn.idoustudio.com/wxareg/company.png?imageView2/1/w/300/h/300)

可以查看创建小程序的申请记录

![setemail](https://blogcdn.idoustudio.com/wxareg/record.png?imageView2/1/w/300/h/300)

## 问题解答

Q：使用第三方平台创建的小程序，还需要申请微信认证吗？

A：使用第三方平台创建的小程序，默认是已认证的小程序，具备企业认证小程序的完整功能，你只需要在验证完法人身份后给小程序设置登录邮箱，后续操作与其他方式申请的小程序一致。

Q：别人用我的企业信息申请创建小程序怎么办？

A：首先别人要知道你的企业信息，其次别人要知道你的手机号和微信号，就算别人知道了全部信息，提交创建小程序申请后，只有你的微信会收到创建小程序的通知，你可以选择不处理，24 小时后申请会自动失效。别人没办法用你的信息创建不属于你的小程序。

Q：我可以在“极速申请”小程序申请创建多少个小程序？

A：目前暂时没有限制，你可以添加任意多个属于你的企业，通过企业资质创建任意多个小程序。但是有个原则，小程序申请只能一个一个创建，必须要法人在微信端提交认证后，才能完成小程序的创建，在此期间，继续提交申请会提示失败。

Q：我有自己的第三方平台，可以实现类似的功能吗？

A：目前使用的第三方平台方是”任想程序“，如果你需要用自己的微信第三方平台来创建小程序，请联系微信：idoubicc，咨询定制开发。

## 功能体验

![qrcode](https://blogcdn.idoustudio.com/wxareg/qrcode.jpg)
