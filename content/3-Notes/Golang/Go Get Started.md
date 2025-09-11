---
title: "Go Get Started"
date: 2022-07-20T20:10:22+08:00
tags: ["Go"]
draft: false
---
# Go语言基础

-   版本管理
    -   gvm: [https://github.com/moovweb/gvm](https://github.com/moovweb/gvm)
    - [使用 GVM 工具管理 Go 版本](https://segmentfault.com/a/1190000023193048)
-   golang basic
    -   目标：了解golang基础语法
    -   推荐资料
        -   [https://go.dev/doc/](https://go.dev/doc/) 
        -   [https://github.com/golang/go](https://github.com/golang/go)
        -   [https://golang.org/doc/effective_go.html](https://golang.org/doc/effective_go.html)
        -   Book: 《Go程序设计语言》
    -   推荐IDE: goland / vscode
-   gin框架
    -   目标：了解gin, 实现一个http hello world
    -   推荐资料
        -   [https://gin-gonic.com/](https://gin-gonic.com/) 
        -   [https://github.com/gin-gonic/gin](https://github.com/gin-gonic/gin)
-   protobuf
    -   了解protobuf的文件定义和编译，跑通一个hello world
    -   推荐资料
        -   [https://github.com/protocolbuffers/protobuf](https://github.com/protocolbuffers/protobuf)
-   grpc
    -   了解grpc和grpc gateway
    -   推荐资料
        -   [https://grpc.io/docs/](https://grpc.io/docs/)


## 版本管理
### GVM简介
GVM 是 Go Version Manager，可用于管理Go的版本，主要特性有：
	-   管理 Go 的多个版本，包括安装、卸载和指定使用 Go 的某个版本；
	-   查看官方所有可用的 Go 版本，同时可以查看本地已安装和默认使用的 Go 版本；
	-   管理多个 GOPATH，并可编辑 Go 的环境变量；
	-   可将当前目录关联到 GOPATH；
	-   可以查看 GOROOT 下的文件差异。
### 安装
```sh
bash < <(curl -s -S -L 
https://raw.githubusercontent.com/moovweb/gvm/master/binscripts/gvm-installer)
```
### 使用GVM
```sh
➜  ~ gvm     
Usage: gvm [command]

Description:
  GVM is the Go Version Manager

Commands:
  version    - print the gvm version number
  get        - gets the latest code (for debugging)
  use        - select a go version to use (--default to set permanently)
  diff       - view changes to Go root
  help       - display this usage text
  implode    - completely remove gvm
  install    - install go versions
  uninstall  - uninstall go versions
  cross      - install go cross compilers
  linkthis   - link this directory into GOPATH
  list       - list installed go versions
  listall    - list available versions
  alias      - manage go version aliases
  pkgset     - manage go packages sets
  pkgenv     - edit the environment for a package set
```

