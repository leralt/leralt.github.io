---
title: "GVM"
tags: ["Go"]
date: 2022-07-20T20:11:20+08:00
---
# GVM
> https://github.com/moovweb/gvm

## 1.GVM简介
GVM 是 Go Version Manager，可用于管理Go的版本，主要特性有：
	-   管理 Go 的多个版本，包括安装、卸载和指定使用 Go 的某个版本；
	-   查看官方所有可用的 Go 版本，同时可以查看本地已安装和默认使用的 Go 版本；
	-   管理多个 GOPATH，并可编辑 Go 的环境变量；
	-   可将当前目录关联到 GOPATH；
	-   可以查看 GOROOT 下的文件差异。
## 2.安装
```bash
bash < <(curl -s -S -L https://raw.githubusercontent.com/moovweb/gvm/master/binscripts/gvm-installer)
```
如果你使用的是zsh，把bash换成zsh即可。
如果出现网络问题，可以多尝试几次。
运行`gvm version`检查是否安装成功。如果提示
```bash
Could not find bison
  linux: apt-get install bison
ERROR: Missing requirements.
```
只需要按照提示，安装包文件即可。
## 3.使用GVM
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
如安装1.18.4版本的go
```bash
gvm install go1.18.4
```
安装 Go 时可以指定其他选项:
```bash
Usage: gvm install [version] [options]
    -s,  --source=SOURCE      Install Go from specified source.
    -n,  --name=NAME          Override the default name for this version.
    -pb, --with-protobuf      Install Go protocol buffers.
    -b,  --with-build-tools   Install package build tools.
    -B,  --binary             Only install from binary.
          --prefer-binary      Attempt a binary install, falling back to source.
    -h,  --help               Display this message.
```


## golang安装和vscode go环境配置
### 下载go二进制文件
最新的软件在https://go.dev/dl/上提供，在终端可以使用如下的命令安装Ubuntu版本的go。
```
wget https://go.dev/dl/go1.18.4.linux-amd64.tar.gz
```
解压缩下载的文件，然后安装到系统中的所需位置。但是通常遵循文档最好将其安装在 `/ user / local / go` 下。在终端中运行以下命令进行安装。
```
sudo tar -xvf go1.18.4.linux-amd64.tar.gz
sudo mv go /usr/local
```
### 设置环境变量
`GOROOT` 是 Go 在机器中安装的路径
`GOPATH` 是工作目录的位置。
根据你的shell环境将该配置添加到`.bashrc`或`.zshrc`文件中。
```
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export PATH=$GOPATH/bin:$GOROOT/bin:$PATH
```
更新你的配置文件或重启终端来使配置生效：
```
source ~/.bashrc
```
运行`go version`来检查是否安装成功。

## 配置vscode
安装好go之后，在vscode中搜索并下载插件Go，下载完成后，使用快捷键`Ctrl+Shift+P`打开命令界面，输入`Go:Install/Update Tools`，全选并点击ok进行安装，如果安装出现错误，有可能是网络原因，在终端配置国内代理
```
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.cn,direct
```
重启vscode再尝试安装，这时就可以安装成功了。