Go基础

> 参考文档： [Go语言的主要特征 · Go语言中文文档](https://www.topgoer.com/go基础/Go语言的主要特征.html) 

> [Learn Go with tests](https://studygolang.gitbook.io/learn-go-with-tests)

## go环境

[从零开始搭建Go语言开发环境 - 李文周的博客](https://www.liwenzhou.com/posts/Go/install_go_dev/)

## 标识符和关键字

https://www.liwenzhou.com/posts/Go/01_var_and_const/

## Go数据类型

[Go语言基础之基本数据类型 - 李文周的博客](https://www.liwenzhou.com/posts/Go/02_datatype/)

## 流程控制

[Go语言基础之流程控制 - 李文周的博客](https://www.liwenzhou.com/posts/Go/04_basic/)

## 运算符

[Go语言基础之运算符 - 李文周的博客](https://www.liwenzhou.com/posts/Go/03_operators/)

## 数组

[Go语言基础之数组 - 李文周的博客](https://www.liwenzhou.com/posts/Go/05_array/)

## 切片

[Go语言基础之切片 - 李文周的博客](https://www.liwenzhou.com/posts/Go/06_slice/)

## 指针

[Go语言基础之指针 - 李文周的博客](https://www.liwenzhou.com/posts/Go/07_pointer/)

## Map

https://www.liwenzhou.com/posts/Go/08_map/

## Defer和异常

[延迟调用(defer) · Go语言中文文档](https://www.topgoer.com/函数/延迟调用defer.html)

1. `recover()`必须搭配`defer`使用。
2. `defer`一定要在可能引发`panic`的语句之前定义。

## 接口

[接口 · Go语言中文文档](https://www.topgoer.com/面向对象/接口.html)

## Package

[Go语言基础之包 - 李文周的博客](https://www.liwenzhou.com/posts/Go/11-package/)

## 文件操作

[Go语言文件操作 - 李文周的博客](https://www.liwenzhou.com/posts/Go/go_file/)

## Goroutine

[Goroutine-地鼠文档](https://www.topgoer.cn/docs/golang/chapter09-2)

## Chan

[Channel-地鼠文档](https://www.topgoer.cn/docs/golang/chapter09-4)

## goTest

[Go语言基础之单元测试 - 李文周的博客](https://www.liwenzhou.com/posts/Go/unit-test/)

## Mysql

[Go语言操作MySQL - 李文周的博客](https://www.liwenzhou.com/posts/Go/go_mysql/)

[go操作MySQL-地鼠文档](https://www.topgoer.cn/docs/golang/chapter10-1)

## Redis

[Redis介绍-地鼠文档](https://www.topgoer.cn/docs/golang/chapter10-2-1)

## Go mod

[Go语言之依赖管理 - 李文周的博客](https://www.liwenzhou.com/posts/Go/go_dependency/)

常用的`go mod`命令如下：

```go
go mod download    下载依赖的module到本地cache（默认为$GOPATH/pkg/mod目录）
go mod edit        编辑go.mod文件
go mod graph       打印模块依赖图
go mod init        初始化当前文件夹, 创建go.mod文件
go mod tidy        增加缺少的module，删除无用的module
go mod vendor      将依赖复制到vendor下
go mod verify      校验依赖
go mod why         解释为什么需要依赖
```

如果只是想修改`go.mod`文件中的内容，那么可以运行`go mod edit -droprequire=package path`，比如要在`go.mod`中移除`golang.org/x/text`包，可以使用如下命令：

```Bash
go mod edit -droprequire=golang.org/x/text 
```

关于`go mod edit`的更多用法可以通过`go help mod edit`查看。

## Context

[Go标准库Context - 李文周的博客](https://www.liwenzhou.com/posts/Go/go_context/)
