---
title: protobuf快速上手指南
categories:
  - 开发笔记
tags:
  - quick start
  - protobuf
date: 2017-12-02 14:08:15
---

## 什么是 protobuf

Google Protocol Buffer( 简称 Protobuf) 是 Google 公司内部的混合语言数据标准。

Protocol Buffers 是一种轻便高效的结构化数据存储格式，可以用于结构化数据串行化，或者说序列化。它很适合做数据存储或 RPC 数据交换格式。可用于通讯协议、数据存储等领域的语言无关、平台无关、可扩展的序列化结构数据格式。

## 如何安装 protobuf

- 在 github 获取 protobuf 源码，windows 系统可以直接下载 exe 文件：https://github.com/google/protobuf/releases

* macos 和 linux 环境使用源码进行安装的步骤

```shell
# 获取源码包
wget https://github.com/google/protobuf/archive/v3.5.0.tar.gz

# 解压缩并进入源码目录
tar -zxvf v3.5.0.tar.gz
cd protobuf-3.5.0

# 生成configure文件
./autogen.sh

# 编译安装
./configure
make
make check
make install
```

> 在执行`./autogen.sh`过程中可能会因缺乏 automake 依赖库而报错：**_autoreconf: failed to run aclocal: No such file or directory_**，要解决此错误，在 linux 系统可以通过`sudo yum install automake`或者`sudo apt-get install automake`安装 automake，在 macos 系统可以通过`brew install automake`安装 automake。

- 检测 protobuf 是否已成功安装

命令行执行`protoc --version`。windows 系统需要将 protoc.exe 文件所在的目录添加到环境变量。

## 如何使用 protobuf

protobuf 支持跨语言使用，很多主流的开发语言都有 protobuf 支持库，我们用 golang 来演示如何使用 protobuf 进行数据序列化。

- 在 golang 中安装 protobuf 相关的库

```go
go get -u github.com/golang/protobuf/{protoc-gen-go,proto}
```

- 编写.proto 文件

\$GOPATH/src/test/protobuf/pb/user.proto

```proto
syntax = "proto3";
package pb;

message user {
    int32 id = 1;
    string name = 2;
}

message multi_user {
    repeated user users = 1;
}
```

- 基于.proto 文件生成数据操作代码

```proto
protoc --go_out=. user.proto
```

执行命令完成，在与`user.proto`文件同级的目录下生成了一个`user.pb.go`文件

- 数据序列化演示
  \$GOPATH/src/test/protobuf/main.go

```go
package main

import (
    "log"
    "test/protobuf/pb"

    "github.com/golang/protobuf/proto"
)

func main() {
    user1 := pb.User{
        Id:   *proto.Int32(1),
        Name: *proto.String("Mike"),
    }

    user2 := pb.User{
        Id:   2,
        Name: "John",
    }

    users := pb.MultiUser{
        Users: []*pb.User{&user1, &user2},
    }

    // 序列化数据
    data, err := proto.Marshal(&users)
    if err != nil {
        log.Fatalln("Marshal data error: ", err)
    }
    println(users.Users[0].GetName()) // output: Mike

    // 对已序列化的数据进行反序列化
    var target pb.MultiUser
    err = proto.Unmarshal(data, &target)
    if err != nil {
        log.Fatalln("Unmarshal data error: ", err)
    }
    println(target.GetUsers()[1].Name) // output: John

}
```

## 相关扩展

sublime 插件：Protobuf Syntax Hightlighting

## 参考资料

- [Protocol Buffers 简明教程](http://ginobefunny.com/post/learning_protobuf/)
- [linux 下安装 google protobuf（详细）](http://blog.csdn.net/xiexievv/article/details/47396725)
- [Golang 序列化之 ProtoBuf](http://www.jianshu.com/p/1a3f1c3031b5)
