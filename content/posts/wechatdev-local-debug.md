---
title: 「微信开发」本地调试指南
date: 2019-04-18 22:20:10
tags:
---

## 前言

在微信公众号的开发模式下，接口配置的URL必须是一个外网地址，这就要求我们在开发调试的时候，必须登录服务器去打一些断点信息，经常一言不合就出现“该公众号提供的服务出现故障，请稍后再试”这种错误提示。

我们希望能有一种解决方案，可以把一个外网地址映射到本地的开发环境，能够实时捕捉到微信公众号发送过来的调试信息，便于我们快速定位问题，完成功能开发。

网上有很多解决方案，比如使用花生壳、ngrok等产品提供的内网穿透技术，实现外网域名到本地开发环境的映射，详情可自行Google。

本文介绍一种对开发者友好，相对简洁的本地开发调试方式。

## 微信本地开发调试

- 准备工作

1. 一个外网域名
2. 一台有公网ip的服务器

- 把外网域名解析到服务器

例如：`wx.idoubi.cc 119.29.201.62`

- 修改服务器sshd配置

```bash
# 打开配置文件
vi /etc/ssh/sshd_config

# 修改配置参数
GatewayPorts yes

# 重启sshd服务
service sshd restart
```

- 本地连接服务器

```bash
# 打开终端，通过ssh隧道连接服务器
ssh -NTf -R 8089:127.0.0.1:8080 root@119.29.201.62
```

- 在服务器查看本地连接的端口是否已监听

```bash
telnet localhost 8089
```

- 在服务器nginx设置域名转发

```sh
server {
    listen       80;
    server_name wx.idoubi.cc;
    location / {
        proxy_pass      http://127.0.0.1:8089;
        proxy_set_header host $http_host;
    }
}
```

- 在本地配置开发路径

```nginx
server {
    listen 8080;
    server_name 127.0.0.1;
    root /data/php/wechat;
    index index.html index.htm index.php;

    location ~ [^/]\.php(/|$) {
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_index index.php;
        include fastcgi.conf;
    }
}
```

- 接入调试

经过上述配置，我们通过外网访问`http://wx.idoubi.cc/index.php`，就能把请求转发到本地的`http://127.0.0.1:8080/index.php`，对应的本地文件地址为`/data/php/wechat/index.php`，所以我们只需要在这个`index.php`文件中编写响应微信服务器请求的代码，就可以在本地进行微信开发调试了。

