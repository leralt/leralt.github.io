---
title: "gRPC"
tags: ["Go","gRPC"]
date: 2022-07-21T11:07:03+08:00
---
## 概述
开始 gRPC 了，这篇文章学习使用 gRPC，输出一个 Hello World。
-   用 Go 实现 gRPC 的服务端。
-   用 Go 实现 gRPC 的客户端。
gRPC 支持 4 类服务方法，咱们这次实现 单项 RPC 和 服务端流式 RPC。
## 四类服务方法
**单项 RPC**
服务端发送一个请求给服务端，从服务端获取一个应答，就像一次普通的函数调用。
```scss
rpc SayHello(HelloRequest) returns (HelloResponse){}
```
**服务端流式 RPC**
客户端发送一个请求给服务端，可获取一个数据流用来读取一系列消息。客户端从返回的数据流里一直读取直到没有更多消息为止。
```scss
rpc LotsOfReplies(HelloRequest) returns (stream HelloResponse){}
```
**客户端流式 RPC**
客户端用提供的一个数据流写入并发送一系列消息给服务端。一旦客户端完成消息写入，就等待服务端读取这些消息并返回应答。
```scss
rpc LotsOfGreetings(stream HelloRequest) returns (HelloResponse) {}
```
**双向流式 RPC**
两边都可以分别通过一个读写数据流来发送一系列消息。这两个数据流操作是相互独立的，所以客户端和服务端能按其希望的任意顺序读写，例如：服务端可以在写应答前等待所有的客户端消息，或者它可以先读一个消息再写一个消息，或者是读写相结合的其他方式。每个数据流里消息的顺序会被保持。
```scss
rpc BidiHello(stream HelloRequest) returns (stream HelloResponse){}
```
## 安装
**安装 Go protobuf 插件**
```go
go get -u github.com/golang/protobuf/proto
go get -u github.com/golang/protobuf/protoc-gen-go
```
**安装 grpc-go**
```bash
go get -u google.golang.org/grpc
```
## 写个 Hello World 服务
-   编写服务端 `.proto` 文件
-   生成服务端 `.pb.go` 文件并同步给客户端
-   编写服务端提供接口的代码
-   编写客户端调用接口的代码
**目录结构**
```go
├─ hello  -- 代码根目录
│  ├─ go_client
│     ├── main.go
│     ├── hello
│        ├── hello.pb.go
│        ├── hello_grpc.pb.go
│  ├─ go_server
│     ├── main.go
│     ├── controller
│         ├── hello_controller
│            ├── hello_server.go
│     ├── hello
│        ├── hello.pb.go
│        ├── hello.proto
│        ├── hello_grpc.pb.go
```
这样创建目录是为了 go_client 和 go_server 后期可以拆成两个项目。
**编写服务端 hello.proto 文件**
```java
syntax = "proto3"; // 指定 proto 版本
package hello;     // 指定包名
// 定义 Hello 服务
service Hello {
	// 定义 SayHello 方法
	rpc SayHello(HelloRequest) returns (HelloResponse) {}

	// 定义 LotsOfReplies 方法
	rpc LotsOfReplies(HelloRequest) returns (stream HelloResponse){}
}

// HelloRequest 请求结构
message HelloRequest {
	string name = 1;
}

// HelloResponse 响应结构
message HelloResponse {
    string message = 1;
}

```
了解更多 Protobuf 语法，请查看：
[https://developers.google.com/protocol-buffers/](https://developers.google.com/protocol-buffers/)
### 生成服务端 `.pb.go`
Go语言的`protobuf`插件和`runtime library`有过2个版本：
-   第1个版本包含有插件`proto-gen-go`，可以生成`xx.pb.go`和`xx_grpc.pb.go`。Go工程里导入该版本的`protobuf runtime`的方式如下：
    ```go
    import "github.com/golang/protobuf"
    ```
-   第2个版本同样包含有插件`proto-gen-go`。不过该项目的`proto-gen-go`从`v1.20`版本开始，不再支持生成gRPC服务定义，也就是`xx_grpc.pb.go`文件。要生成gRPC服务定义需要使用`grpc-go`里的`progo-gen-go-grpc`插件。Go工程里导入该版本的`protobuf runtime`的方式如下：
    ```go
    import "google.golang.org/protobuf"
    ```
推荐使用第2个版本，对protobuf的API做了优化和精简，并且把工程界限分清楚了：
-   第一，把`protobuf`的Go实现都放在protobuf的项目里，而不是放在golang语言项目下面。
-   第二，把`gRPC`的生成，放在`grpc-go`项目里，而不是和`protobuf runtime`混在一起。
有的老项目可能使用了第1个版本的`protobuf runtime`，在老项目里开发新功能的时候也可以使用第2个版本`protobuf runtime`，支持2个版本在一个Go项目里共存。但是要**注意**：一个项目里同时使用2个版本必须保证第一个版本的版本号不低于`v1.4`。
### 安装`protoc`编译器的Go语言插件
`protoc-gen-go`插件：用于生成`xx.pb.go`文件
```go
go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
```
`protoc-gen-go-grpc`插件：用于生成`xx_grpc.pb.go`文件
```go
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest
```
**注意**：有的教程可能只让你安装`protoc-gen-go`，没有安装`protoc-gen-go-grpc`，那有2种情况：
-   使用的是第1个版本`github.com/golang/protobuf`的`protoc-gen-go`插件。
-   使用的是第2个版本`google.golang.org/protobuf`的`protoc-gen-go`插件并且`protoc-gen-go`版本号低于`v1.20`。从`v1.20`开始，第2个版本的`protoc-gen-go`插件不再支持生成gRPC服务定义。下面是官方说明：
> The v1.20 [`protoc-gen-go`](https://link.juejin.cn?target=https%3A%2F%2Flink.segmentfault.com%2F%3Fenc%3DJWYOMIxaYRjHUlvfZfd92Q%253D%253D.2X4mQDlcsI%252FhDkHvfiKbUVpYHzIsG%252FfpHs4WNI99Md%252FW8QLz%252FRoNVj%252FmCbYbuO9y9dpW1gr8anNCYfZEoJwauQ%253D%253D "https://link.segmentfault.com/?enc=JWYOMIxaYRjHUlvfZfd92Q%3D%3D.2X4mQDlcsI%2FhDkHvfiKbUVpYHzIsG%2FfpHs4WNI99Md%2FW8QLz%2FRoNVj%2FmCbYbuO9y9dpW1gr8anNCYfZEoJwauQ%3D%3D") does not support generating gRPC service definitions. In the future, gRPC service generation will be supported by a new `protoc-gen-go-grpc` plugin provided by the Go gRPC project.
> The `github.com/golang/protobuf` version of `protoc-gen-go` continues to support gRPC and will continue to do so for the foreseeable future.


在项目目录使用`protoc`命令编译`.proto`文件，生成新的`hello.pb.go`和`hello_grpc.pb.go`文件。命令如下：
```css
protoc --go_out=. --go_opt=paths=source_relative \
    --go-grpc_out=. --go-grpc_opt=paths=source_relative \
    hello/hello.proto
```

同时将生成的文件复制到客户端一份。
查看更多命令参数，执行 protoc，查看 OPTION 。
**编写服务端提供接口的代码**
```go
// hello_server.go
package hello_controller

import (
	"fmt"
	"golang.org/x/net/context"
	"hello/go_server/proto/hello"
)

type HelloController struct{}

func (h *HelloController) SayHello(ctx context.Context, in *hello.HelloRequest) (*hello.HelloResponse, error) {
	return &hello.HelloResponse{Message : fmt.Sprintf("%s", in.Name)}, nil
}

func (h *HelloController) LotsOfReplies(in *hello.HelloRequest, stream hello.Hello_LotsOfRepliesServer)  error {
	for i := 0; i < 10; i++ {
		stream.Send(&hello.HelloResponse{Message : fmt.Sprintf("%s %s %d", in.Name, "Reply", i)})
	}
	return nil
}
```
服务端其实是实现协议中的接口，即实现所有方法
```go
type HelloServer interface {
	SayHello(context.Context, *HelloRequest) (*HelloResponse, error)
	LotsOfReplies(*HelloRequest, Hello_LotsOfRepliesServer) error
	mustEmbedUnimplementedHelloServer()
}
```
很多教程实现了mustEmbedUnimplementedHelloServer 这个方法，但是由于是小写， 同目录下是好的，跨了目录就会有问题。
应该直接嵌入到实现的结构体中：
```go
type server struct {
	pb.UnimplementedGreeterServer
}
```
```go
// main.go
package main

import (
	"log"
	"net"
	"hello/go_server/proto/hello"
	"hello/go_server/controller/hello_controller"
	"google.golang.org/grpc"
)

const (
	Address = "0.0.0.0:9090"
)

func main() {
	listen, err := net.Listen("tcp", Address)
	if err != nil {
		log.Fatalf("Failed to listen: %v", err)
	}

	s := grpc.NewServer()

	// 服务注册
	hello.RegisterHelloServer(s, &hello_controller.HelloController{})

	log.Println("Listen on " + Address)

	if err := s.Serve(listen); err != nil {
		log.Fatalf("Failed to serve: %v", err)
	}
}
```
运行：
```go
go run main.go
2019/07/28 17:51:20 Listen on 0.0.0.0:9090
```
**编写客户端请求接口的代码**
```go
package main

import (
	"hello/go_client/proto/hello"
	"io"
	"log"
	"golang.org/x/net/context"
	"google.golang.org/grpc"
)

const (
	// gRPC 服务地址
	Address = "0.0.0.0:9090"
)

func main() {
	conn, err := grpc.Dial(Address, grpc.WithInsecure())
	if err != nil {
		log.Fatalln(err)
	}
	defer conn.Close()
	// 初始化客户端
	c := hello.NewHelloClient(conn)

	// 调用 SayHello 方法
	res, err := c.SayHello(context.Background(), &hello.HelloRequest{Name: "Hello World"})
	if err != nil {
		log.Fatalln(err)
	}

	log.Println(res.Message)

	// 调用 LotsOfReplies 方法
	stream, err := c.LotsOfReplies(context.Background(),&hello.HelloRequest{Name: "Hello World"})
	if err != nil {
		log.Fatalln(err)
	}

	for {
		res, err := stream.Recv()
		if err == io.EOF {
			break
		}

		if err != nil {
			log.Printf("stream.Recv: %v", err)
		}

		log.Printf("%s", res.Message)
	}
}
```
运行：
```yaml
go run main.go
2019/07/28 17:58:13 Hello World
2019/07/28 17:58:13 Hello World Reply 0
2019/07/28 17:58:13 Hello World Reply 1
2019/07/28 17:58:13 Hello World Reply 2
2019/07/28 17:58:13 Hello World Reply 3
2019/07/28 17:58:13 Hello World Reply 4
2019/07/28 17:58:13 Hello World Reply 5
2019/07/28 17:58:13 Hello World Reply 6
2019/07/28 17:58:13 Hello World Reply 7
2019/07/28 17:58:13 Hello World Reply 8
2019/07/28 17:58:13 Hello World Reply 9
```
