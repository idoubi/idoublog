---
title: golang实现RPC的几种方式
categories:
  - 开发笔记
tags:
  - golang
  - RPC
date: 2017-12-03 11:45:26
---

## 什么是RPC

远程过程调用（Remote Procedure Call，缩写为 RPC）是一个计算机通信协议。 该协议允许运行于一台计算机的程序调用另一台计算机的子程序，而程序员无需额外地为这个交互作用编程。 如果涉及的软件采用面向对象编程，那么远程过程调用亦可称作远程调用或远程方法调用。[维基百科：远程过程调用](https://zh.wikipedia.org/wiki/%E9%81%A0%E7%A8%8B%E9%81%8E%E7%A8%8B%E8%AA%BF%E7%94%A8)

用通俗易懂的语言描述就是：RPC允许跨机器、跨语言调用计算机程序方法。打个比方，我用go语言写了个获取用户信息的方法getUserInfo，并把go程序部署在阿里云服务器上面，现在我有一个部署在腾讯云上面的php项目，需要调用golang的getUserInfo方法获取用户信息，php跨机器调用go方法的过程就是RPC调用。

## golang中如何实现RPC

在golang中实现RPC非常简单，有封装好的官方库和一些第三方库提供支持。Go RPC可以利用tcp或http来传递数据，可以对要传递的数据使用多种类型的编解码方式。golang官方的`net/rpc`库使用`encoding/gob`进行编解码，支持`tcp`或`http`数据传输方式，由于其他语言不支持`gob`编解码方式，所以使用`net/rpc`库实现的RPC方法没办法进行跨语言调用。

golang官方还提供了`net/rpc/jsonrpc`库实现RPC方法，JSON RPC采用JSON进行数据编解码，因而支持跨语言调用。但目前的jsonrpc库是基于tcp协议实现的，暂时不支持使用http进行数据传输。

除了golang官方提供的rpc库，还有许多第三方库为在golang中实现RPC提供支持，大部分第三方rpc库的实现都是使用`protobuf`进行数据编解码，根据`protobuf`声明文件自动生成rpc方法定义与服务注册代码，在golang中可以很方便的进行rpc服务调用。

### net/rpc库

下面的例子演示一下如何使用golang官方的`net/rpc`库实现RPC方法，使用`http`作为RPC的载体，通过`net/http`包监听客户端连接请求。

$GOPATH/src/test/rpc/rpc_server.go

```go
package main

import (
    "errors"
    "fmt"
    "log"
    "net"
    "net/http"
    "net/rpc"
    "os"
)

// 算数运算结构体
type Arith struct {
}

// 算数运算请求结构体
type ArithRequest struct {
    A int
    B int
}

// 算数运算响应结构体
type ArithResponse struct {
    Pro int // 乘积
    Quo int // 商
    Rem int // 余数
}

// 乘法运算方法
func (this *Arith) Multiply(req ArithRequest, res *ArithResponse) error {
    res.Pro = req.A * req.B
    return nil
}

// 除法运算方法
func (this *Arith) Divide(req ArithRequest, res *ArithResponse) error {
    if req.B == 0 {
        return errors.New("divide by zero")
    }
    res.Quo = req.A / req.B
    res.Rem = req.A % req.B
    return nil
}

func main() {
    rpc.Register(new(Arith)) // 注册rpc服务
    rpc.HandleHTTP()         // 采用http协议作为rpc载体

    lis, err := net.Listen("tcp", "127.0.0.1:8095")
    if err != nil {
        log.Fatalln("fatal error: ", err)
    }

    fmt.Fprintf(os.Stdout, "%s", "start connection")

    http.Serve(lis, nil)
}
```

上述服务端程序运行后，将会监听本地的8095端口，我们可以实现一个客户端程序，连接服务端并实现RPC方法调用。

$GOPATH/src/test/rpc/rpc_client.go

```go
package main

import (
    "fmt"
    "log"
    "net/rpc"
)

// 算数运算请求结构体
type ArithRequest struct {
    A int
    B int
}

// 算数运算响应结构体
type ArithResponse struct {
    Pro int // 乘积
    Quo int // 商
    Rem int // 余数
}

func main() {
    conn, err := rpc.DialHTTP("tcp", "127.0.0.1:8095")
    if err != nil {
        log.Fatalln("dailing error: ", err)
    }

    req := ArithRequest{9, 2}
    var res ArithResponse

    err = conn.Call("Arith.Multiply", req, &res) // 乘法运算
    if err != nil {
        log.Fatalln("arith error: ", err)
    }
    fmt.Printf("%d * %d = %d\n", req.A, req.B, res.Pro)

    err = conn.Call("Arith.Divide", req, &res)
    if err != nil {
        log.Fatalln("arith error: ", err)
    }
    fmt.Printf("%d / %d, quo is %d, rem is %d\n", req.A, req.B, res.Quo, res.Rem)
}
```

### net/rpc/jsonrpc库

上面的例子我们演示了使用`net/rpc`实现RPC的过程，但是没办法在其他语言中调用上面例子实现的RPC方法。所以接下来的例子我们演示一下使用`net/rpc/jsonrpc`库实现RPC方法，此方式实现的RPC方法支持跨语言调用。

$GOPATH/src/test/rpc/jsonrpc_server.go

```go
package main

import (
    "errors"
    "fmt"
    "log"
    "net"
    "net/rpc"
    "net/rpc/jsonrpc"
    "os"
)

// 算数运算结构体
type Arith struct {
}

// 算数运算请求结构体
type ArithRequest struct {
    A int
    B int
}

// 算数运算响应结构体
type ArithResponse struct {
    Pro int // 乘积
    Quo int // 商
    Rem int // 余数
}

// 乘法运算方法
func (this *Arith) Multiply(req ArithRequest, res *ArithResponse) error {
    res.Pro = req.A * req.B
    return nil
}

// 除法运算方法
func (this *Arith) Divide(req ArithRequest, res *ArithResponse) error {
    if req.B == 0 {
        return errors.New("divide by zero")
    }
    res.Quo = req.A / req.B
    res.Rem = req.A % req.B
    return nil
}

func main() {
    rpc.Register(new(Arith)) // 注册rpc服务

    lis, err := net.Listen("tcp", "127.0.0.1:8096")
    if err != nil {
        log.Fatalln("fatal error: ", err)
    }

    fmt.Fprintf(os.Stdout, "%s", "start connection")

    for {
        conn, err := lis.Accept() // 接收客户端连接请求
        if err != nil {
            continue
        }

        go func(conn net.Conn) { // 并发处理客户端请求
            fmt.Fprintf(os.Stdout, "%s", "new client in coming\n")
            jsonrpc.ServeConn(conn)
        }(conn)
    }
}
```

上述服务端程序启动后，将会监听本地的8096端口，并处理客户端的tcp连接请求。我们可以用golang实现一个客户端程序连接上述服务端并进行RPC调用。

$GOPATH/src/test/rpc/jsonrpc_client.go

```go
package main

import (
    "fmt"
    "log"
    "net/rpc/jsonrpc"
)

// 算数运算请求结构体
type ArithRequest struct {
    A int
    B int
}

// 算数运算响应结构体
type ArithResponse struct {
    Pro int // 乘积
    Quo int // 商
    Rem int // 余数
}

func main() {
    conn, err := jsonrpc.Dial("tcp", "127.0.0.1:8096")
    if err != nil {
        log.Fatalln("dailing error: ", err)
    }

    req := ArithRequest{9, 2}
    var res ArithResponse

    err = conn.Call("Arith.Multiply", req, &res) // 乘法运算
    if err != nil {
        log.Fatalln("arith error: ", err)
    }
    fmt.Printf("%d * %d = %d\n", req.A, req.B, res.Pro)

    err = conn.Call("Arith.Divide", req, &res)
    if err != nil {
        log.Fatalln("arith error: ", err)
    }
    fmt.Printf("%d / %d, quo is %d, rem is %d\n", req.A, req.B, res.Quo, res.Rem)
}
```


### protorpc库

为了实现跨语言调用，在golang中实现RPC方法的时候我们应该选择一种跨语言的数据编解码方式，比如JSON，上述的`jsonrpc`可以满足此要求，但是也存在一些缺点，比如不支持http传输，数据编解码性能不高等。于是呢，一些第三方rpc库都选择采用`protobuf`进行数据编解码，并提供一些服务注册代码自动生成功能。下面的例子我们使用`protobuf`来定义RPC方法及其请求响应参数，并使用第三方的`protorpc`库来生成RPC服务注册代码。

首先，需要安装`protobuf`及`protoc`可执行命令，可以参考此篇文章：[protobuf快速上手指南](http://idoubi.cc/2017/12/02/protobuf%E5%BF%AB%E9%80%9F%E4%B8%8A%E6%89%8B%E6%8C%87%E5%8D%97/)

然后，我们编写一个proto文件，定义要实现的RPC方法及其相关参数。

$GOPATH/src/test/rpc/pb/arith.proto

```protobuf
syntax = "proto3";
package pb;

// 算术运算请求结构
message ArithRequest {
    int32 a = 1;
    int32 b = 2;
}

// 算术运算响应结构
message ArithResponse {
    int32 pro = 1;  // 乘积
    int32 quo = 2;  // 商
    int32 rem = 3;  // 余数
}

// rpc方法
service ArithService {
    rpc multiply (ArithRequest) returns (ArithResponse);    // 乘法运算方法
    rpc divide (ArithRequest) returns (ArithResponse);      // 除法运算方法
}
```

接下来我们需要根据上述定义的`arith.proto`文件生成RPC服务代码。
要先安装`protorpc`库：`go get github.com/chai2010/protorpc`
然后使用`protoc`工具生成代码：`protoc --go_out=plugin=protorpc=. arith.proto`
执行`protoc`命令后，在与`arith.proto`文件同级的目录下生成了一个`arith.pb.go`文件，里面包含了RPC方法定义和服务注册的代码。

基于生成的`arith.pb.go`代码我们来实现一个rpc服务端

$GOPATH/src/test/rpc/protorpc_server.go

```go
package main

import (
    "errors"
    "test/rpc/pb"
)

// 算术运算结构体
type Arith struct {
}

// 乘法运算方法
func (this *Arith) Multiply(req *pb.ArithRequest, res *pb.ArithResponse) error {
    res.Pro = req.GetA() * req.GetB()
    return nil
}

// 除法运算方法
func (this *Arith) Divide(req *pb.ArithRequest, res *pb.ArithResponse) error {
    if req.GetB() == 0 {
        return errors.New("divide by zero")
    }
    res.Quo = req.GetA() / req.GetB()
    res.Rem = req.GetA() % req.GetB()
    return nil
}

func main() {
    pb.ListenAndServeArithService("tcp", "127.0.0.1:8097", new(Arith))
}
```

运行上述程序，将会监听本地的8097端口并接收客户端的tcp连接。

基于`ariti.pb.go`再来实现一个客户端程序。

$GOPATH/src/test/protorpc_client.go

```go
package main

import (
    "fmt"
    "log"
    "test/rpc/pb"
)

func main() {
    conn, err := pb.DialArithService("tcp", "127.0.0.1:8097")
    if err != nil {
        log.Fatalln("dailing error: ", err)
    }
    defer conn.Close()

    req := &pb.ArithRequest{9, 2}

    res, err := conn.Multiply(req)
    if err != nil {
        log.Fatalln("arith error: ", err)
    }
    fmt.Printf("%d * %d = %d\n", req.GetA(), req.GetB(), res.GetPro())

    res, err = conn.Divide(req)
    if err != nil {
        log.Fatalln("arith error ", err)
    }
    fmt.Printf("%d / %d, quo is %d, rem is %d\n", req.A, req.B, res.Quo, res.Rem)
}
```


## 如何跨语言调用golang的RPC方法

上面的三个例子，我们分别使用`net/rpc`、`net/rpc/jsonrpc`、`protorpc`实现了golang中的RPC服务端，并给出了对应的golang客户端RPC调用示例，因为JSON和protobuf是支持多语言的，所以使用`jsonrpc`和`protorpc`实现的RPC方法我们是可以在其他语言中进行调用的。下面给出一个php客户端程序，通过socket连接调用jsonrpc实现的服务端RPC方法。

$PHPROOT/jsonrpc.php

```php
<?php

class JsonRPC {

    private $conn;

    function __construct($host, $port) {
        $this->conn = fsockopen($host, $port, $errno, $errstr, 3);
        if (!$this->conn) {
            return false;
        }
    }

    public function Call($method, $params) {
        if (!$this->conn) {
            return false;
        }
        $err = fwrite($this->conn, json_encode(array(
                'method' => $method,
                'params' => array($params),
                'id'     => 0,
            ))."\n");
        if ($err === false) {
            return false;
        }
        stream_set_timeout($this->conn, 0, 3000);
        $line = fgets($this->conn);
        if ($line === false) {
            return NULL;
        }
        return json_decode($line,true);
    }
}

$client = new JsonRPC("127.0.0.1", 8096);
$args = array('A'=>9, 'B'=>2);
$r = $client->Call("Arith.Multiply", $args);
printf("%d * %d = %d\n", $args['A'], $args['B'], $r['result']['Pro']);
$r = $client->Call("Arith.Divide", array('A'=>9, 'B'=>2));
printf("%d / %d, Quo is %d, Rem is %d\n", $args['A'], $args['B'], $r['result']['Quo'], $r['result']['Rem']);
```


## 其他RPC库

除了上面提到的三种在golang实现RPC的方式外，还有一些其他的rpc库提供了类似的功能，比较出名的有google开源的grpc，但是grpc的初次安装比较麻烦，这里就不做进一步介绍了，有兴趣的可以自己了解。

## 参考资料

- [Go官方库RPC开发指南](http://colobu.com/2016/09/18/go-net-rpc-guide/)
- [go web开发教程-RPC](https://github.com/astaxie/build-web-application-with-golang/blob/master/zh/08.4.md)
- [Go RPC 开发指南](https://smallnest.gitbooks.io/go-rpc-programming-guide/content/)
