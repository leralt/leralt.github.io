---
title: "Gin Web框架"
date: 2022-07-20T18:39:33+08:00
tags: ["Go"]
---
# Gin Web框架
## 安装
1. 下载安装
```
go get -u github.com/gin-gonic/gin
```
2. 导入到代码中
```go
import "github.com/gin-gonic/gin"
```
## 开始
创建一个`example.go`文件，键入以下代码：
```go
package main

import "github.com/gin-gonic/gin"

func main() {
	r := gin.Default()
	r.GET("/", func(c *gin.Context) {
		c.JSON(200, gin.H{
			"message": "Hello World!",
		})
	})
	r.Run() // listen and serve on 0.0.0.0:8080
}
```
1. 首先我们使用gin.Default()生存了一个实例，这个实例就是WSGI应用程序
2. 之后我们用r.Get("/", ...)声明了一个路由，告诉 Gin 什么样的URL 能触发传入的函数，这个函数返回我们想要显示在用户浏览器中的信息。
3. 最后用r.Run()函数让应用运行在本地服务器上，默认的监听端口是8080，可以传入参数设置端口，例如`r.Run(":9999")`即运行在 9999端口。
然后`go run example.go`运行你的代码
在浏览器中访问http://127.0.0.1:8080 ，就可以看到信息被打印出来。
## Gin 特性
-   **快速**：路由不使用反射，基于Radix树^[Radix树，即基数树，也称压缩前缀树，是一种提供key-value存储查找的数据结构。 与Trie不同的是，它对Trie树进行了空间优化，只有一个子节点的中间节点将被压缩。 同样的，Radix树的插入、查询、删除操作的时间复杂度都为O(k)。]，内存占用少。
-   **中间件**：HTTP请求，可先经过一系列中间件处理，例如：Logger，Authorization，GZIP等。这个特性和 NodeJs 的 `Koa` 框架很像。中间件机制也极大地提高了框架的可扩展性。
-   **异常处理**：服务始终可用，不会宕机。Gin 可以捕获 panic，并恢复。而且有极为便利的机制处理HTTP请求过程中发生的错误。
-   **JSON**：Gin可以解析并验证请求的JSON。这个特性对`Restful API`的开发尤其有用。
-   **路由分组**：例如将需要授权和不需要授权的API分组，不同版本的API分组。而且分组可嵌套，且性能不受影响。
-   **渲染内置**：原生支持JSON，XML和HTML的渲染

## 路由（Route）
路由方法有GET、POST、PUT、PATCH、DELETE和OPTIONS，还有Any，可匹配以上任意类型的参数。
### 解析路径参数
有时候我们需要动态的路由，如`/user/:name`，通过调用不同的url来传入不同的name。
```go
r.GET("/user/:name", func(c *gin.Context) {
	name := c.Param("name")
	c.String(http.StatusOK, "Hello %s", name)
})
```
```
$ curl http://127.0.0.1:8080/user/long
Hello long
```
### 获取Query参数
```go
r.GET("/users", func(c *gin.Context) {
	name := c.Query("name")
	role := c.DefaultQuery("role", "teacher")
	c.String(http.StatusOK, "%s is a %s", name, role)
})
```
```
$ curl "http://127.0.0.1:8080/users?name=long&role=student"
long is a student
```
### 获取POST参数
```go
r.POST("/form", func(c *gin.Context) {
	username := c.PostForm("username")
	password := c.DefaultPostForm("password", "123456") //可设默认值
	c.JSON(http.StatusOK, gin.H{
		"username": username,
		"password": password,
	})
})
```
```
$ curl http://localhost:8080/form  -X POST -d 'username=long'
{"password":"123456","username":"long"}
```
### Map参数
```go
r.POST("/post", func(c *gin.Context) {  
	ids := c.QueryMap("ids")  
	names := c.PostFormMap("names")  
  
	c.JSON(http.StatusOK, gin.H{  
		"ids":   ids,  
		"names": names,  
	})  
})
```
```
$ curl -g "http://localhost:8080/post?ids[Jack]=001&ids[Tom]=002" -X POST -d 'names[a]=Sam&names[b]=David'  
{"ids":{"Jack":"001","Tom":"002"},"names":{"a":"Sam","b":"David"}}
```
### 重定向
```go
r.GET("/redirect", func(c *gin.Context) {  
    c.Redirect(http.StatusMovedPermanently, "/")  
})  
  
r.GET("/goindex", func(c *gin.Context) {  
	c.Request.URL.Path = "/"  
	r.HandleContext(c)  
})
```
```
$ curl -i http://localhost:8080/redirect  
HTTP/1.1 301 Moved Permanently  
Content-Type: text/html; charset=utf-8  
Location: /  
Date: Thu, 08 Aug 2019 17:22:14 GMT  
Content-Length: 36  
  
<a href="/">Moved Permanently</a>.  
  
$ curl "http://localhost:8080/goindex"  
Hello World!
```
### 分组路由(Grouping Routes)
```go
// group routes 分组路由  
defaultHandler := func(c *gin.Context) {  
	c.JSON(http.StatusOK, gin.H{  
		"path": c.FullPath(),  
	})  
}  
// group: v1  
v1 := r.Group("/v1")  
{  
	v1.GET("/posts", defaultHandler)  
	v1.GET("/series", defaultHandler)  
}  
// group: v2  
v2 := r.Group("/v2")  
{  
	v2.GET("/posts", defaultHandler)  
	v2.GET("/series", defaultHandler)  
}
```
```
$ curl http://localhost:8080/v1/posts  
{"path":"/v1/posts"}  
$ curl http://localhost:8080/v2/posts  
{"path":"/v2/posts"}
```