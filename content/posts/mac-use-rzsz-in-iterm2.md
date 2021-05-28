---
title: 「Mac」在iterm2中使用rzsz命令进行文件传输
date: 2019-05-29 09:24:19
tags:
---

在 Mac 系统中，我们常用的终端工具是 iterm2，有时候需要通过 ssh 连接上服务器，并在服务器上面通过 rz、sz 命令上传文件或下载文件到本地。要实现此场景下的需求，我们需要在服务器和本地分别配置 lrzsz 命令行工具。

## 服务器上安装 lrzsz

两种安装方式二选一

- yum 安装

```
yum install lrzsz
```

- 源码编译安装

```
cd
wget http://www.ohse.de/uwe/releases/lrzsz-0.12.20.tar.gz
tar zxvf lrzsz-0.12.20.tar.gz && cd lrzsz-0.12.20
./configure && make && make install
```

创建软连接

```
ln -s /usr/local/bin/lrz rz
ln -s /usr/local/bin/lsz sz
```

## Mac 系统安装 lrzsz

```
brew install lrzsz
```

## iterm2 传输配置

```
cd /usr/local/bin
wget https://raw.githubusercontent.com/aikuyun/iterm2-zmodem/master/iterm2-send-zmodem.sh
wget https://raw.githubusercontent.com/aikuyun/iterm2-zmodem/master/iterm2-recv-zmodem.sh
chmod +x /usr/local/bin/iterm2-send-zmodem.sh /usr/local/bin/iterm2-recv-zmodem.sh
```

点击`iTerm2/Preferences/Profiles/default/Advanced/Edit`

添加配置

| Regular Expression                | Action               | Parameters                           | Instant |
| --------------------------------- | -------------------- | ------------------------------------ | ------- |
| rz waiting to receive.\\*\\*B0100 | Run Silent Coprocess | /usr/local/bin/iterm2-send-zmodem.sh | checked |
| \\*\\*B00000000000000             | Run Silent Coprocess | /usr/local/bin/iterm2-recv-zmodem.sh | checked |

![](http://blogcdn.idoustudio.com/iterm2rzsz.png)

配置完成后，在服务器上使用 rz、sz 命令即可进行文件的上传下载操作。
