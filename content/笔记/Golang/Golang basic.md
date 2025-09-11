---
title: "Golang basic"
tags: ["Go"]
date: 2022-07-20T14:34:49+08:00
categories: ["学习"]
---
# Go语言基础
> 参考书籍：[《The Go Programming Language》](https://books.studygolang.com/gopl-zh/)
> 入门视频：[go语言快速入门](https://www.bilibili.com/video/BV1fD4y1m7TD?spm_id_from=333.999.0.0)
## 命名
名字的开头字母的大小写决定了名字在包外的可见性，大写表示导出
## 声明
Go语言主要有四种类型的声明语句：var、const、type、func，分别对应变量、常量、类型和函数实体对象的声明。
## 变量
变量的声明方式：
```
var 变量名称 类型 = 表达式
```
如:
```go
var s string = "Hello World"
```
### 简短变量声明
在函数内部，有一种称为简短变量声明语句的形式可用于声明和初始化局部变量。它以“名字 := 表达式”形式声明变量，变量的类型根据表达式来自动推导。
```go
i := 100
```
## 数据类型
- 布尔型：true or false
- 数字类型： 整型int和浮点型float32、float64
- 字符串类型：Go中使用UTF-8编码
- 派生类型
	- 指针类型(Pointer)
	- 数组类型
	- 结构体(struct)
	- channel
	- 函数类型
	- 切片类型
	- 接口类型(interface)
	- Map类型
## 值类型和引用类型
所有像int、float、bool、string这些基本类型都属于值类型，使用这些变量直接指向存在内存中的值。
当使用等号=将一个变量的值赋给另一个变量时，实际上是在内存中进行了拷贝。
切片为引用类型，