---
title: "Protobuf"
tags: ["Go","Protobuf","grpc"]
date: 2022-07-20T20:11:34+08:00
---

# Go Protobuf 入门
## 简介
protobuf 即 Protocol Buffers，是一种轻便高效的结构化数据存储格式，与语言、平台无关，可扩展可序列化。protobuf 性能和效率大幅度优于 JSON、XML 等其他的结构化数据格式。protobuf 是以二进制方式存储的，占用空间小，但也带来了可读性差的缺点。protobuf 在通信协议和数据存储等领域应用广泛。例如著名的分布式缓存工具 [Memcached](https://memcached.org/) 的 Go 语言版本[groupcache](https://github.com/golang/groupcache) 就使用了 protobuf 作为其 RPC 数据格式。
Protobuf 在 `.proto` 定义需要处理的结构化数据，可以通过 `protoc` 工具，将 `.proto` 文件转换为 C、C++、Golang、Java、Python 等多种语言的代码，兼容性好，易于使用。
## 安装
### protoc
从Protobuf Releases 下载最新版本的发布包安装，如果是Ubuntu，可以安装如下的方式安装（以3.21.2为例）
```bash
wget https://github.com/protocolbuffers/protobuf/releases/download/v21.2/protoc-21.2-linux-x86_64.zip
sudo 7z x protoc-21.2-linux-x86_64.zip -o/usr/local
```
如果不想安装在 /usr/local 目录下，可以解压到其他的其他，并把解压路径下的 bin 目录 加入到环境变量即可。
如果能正常显示版本，则代表安装成功。
```bash
$ protoc --version
libprotoc 3.21.2
```
如果提示
```bash
bash: /usr/local/bin/protoc: 权限不够
```
则是protoc缺少执行权限，只需要运行下面的命令即可
```bash
$ sudo chmod +x /usr/local/bin/protoc
```
### protoc-gen-go
在 Golang 中使用 protobuf，还需要安装 protoc-gen-go，这个工具用来将 `.proto` 文件转换为 Golang 代码。
```bash
$ go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
```
## 定义消息类型
创建一个简单的示例， `student.proto`
```proto
syntax = "proto3";  
package main;  
  
// this is a comment  
message Student {  
  string name = 1;  
  bool male = 2;  
  repeated int32 scores = 3;  
}
```
### package
定义proto的包名，包名可以避免对message 类型之间的名字冲突，同名的Message可以通过package进行区分。
在没有为特定语言定义`option xxx_package`的时候，它还可以用来生成特定语言的包名，比如Java package, go package。
在当前目录下执行：
```bash
$ protoc --go_out=. *.proto
$ ls
student.pb.go student.proto
```
即是，将该目录下的所有的 .proto 文件转换为 Go 代码，我们可以看到该目录下多出了一个 Go 文件 _student.pb.go_。这个文件内部定义了一个结构体 Student，以及相关的方法：
## 一些问题

> 问题一：protoc 无法编译.proto文件

```bash
protoc-gen-go: program not found or is not executable
Please specify a program using absolute path or make sure the program is available in your PATH system variable
--go_out: protoc-gen-go: Plugin failed with status code 1.
```

解决办法：
[protoc-gen-go 安装踩坑记录](https://chowdera.com/2021/12/20211202002706731i.html)
> 问题二：上面方法生成的student.pb.go文件，package名为 __，无法直接使用。

解决办法：
将
```proto
option go_package =".";
```
修改为
```proto
option go_package=".;mian";
```
引号中的分号前为生成文件路径，分号后为包名，如果没有后半部分，protoc会默认将路径名作为包名，并将其中的`.`转换为`__`，这就是为什么生成的包名为`__`。

>参考链接
>
>https://developers.google.com/protocol-buffers/docs/overview
>
>https://geektutu.com/post/quick-go-protobuf.html