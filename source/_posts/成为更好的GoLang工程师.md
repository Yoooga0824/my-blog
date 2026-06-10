---
title: 成为更好的GoLang工程师
date: 2026-06-10 20:56:37
cover: /images/20260610.jpg
tags:
    - 技术
    - GoLang
categories:
    - Let's go
feature: true
---

# 成为更好的GoLang工程师
## 1. GoLang环境安装
> 访问官网 https://go.dev/dl/

在Linux终端
```bash
cd
wget https://go.dev/dl/go1.26.4.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.26.4.linux-amd64.tar.gz

nano ~/.bashrc
// 在文件末尾添加以下三行：
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
// Ctrl + X 和 Ctrl + O

source ~/.bashrc
go version

// 配置国内镜像源加速
echo "export GOPROXY=https://goproxy.cn,direct" >> ~/.bashrc
source ~/.bashrc
```

## 2. 创建GOPATH目录结构
```bash
mkdir -p $GOPATH/{src,pkg,bin}
```
**为什么需要 GOPATH？**
GOPATH 是 Go 语言早期版本的工作区根目录。虽然现在有了 Go Modules（可以在任意目录工作），但设置好 GOPATH 仍然是一种好习惯，也兼容一些老工具.

## 3. 上手Go代码
#### 3.1 Hello GO！
```go
package main  // 声明包名，每个源文件都属于一个包

// import "fmt" // 导入fmt包，用于打印输出
// import "time"

import (
	"fmt"
	"time"
)

func main() {
	fmt.Println("Hello Go!")
	time.Sleep(1 * time.Second)
}
```

```bash
go run Hello.go 等同于
go build Hello.go 加上 ./Hello
```
#### 3.2 变量声明
```go
// 四种变量声明方式
package main
import (
	"fmt"
)

//声明全局变量，一二三都可以，四不可以，只能用在函数体内
var Ga = 100

func main() {
	//方法一：声明一个变量,默认的值是0
	var a int
	fmt.Println("a =", a)

	//方法二：声明一个变量，初始化一个值
	var b int = 100
	fmt.Println("b =", b)

	//方法三：在初始化的时候，省略数据类型
	var c = 100;
	fmt.Println("c =", c)

	//方法四：常用，省去var关键字，自动匹配
	e := "abcd"
	e = "bcde"
	fmt.Println("e =", e)
	fmt.Printf("Type of e = %T\n", e)

	//打印全局变量
	fmt.Println("Ga =", Ga)

	//声明多个变量
	var xx, yy int = 100, 200
	var (
		vv int = 100
		jj bool = true
	)
	fmt.Println("xx =", xx)
	fmt.Println("yy =", yy)
	fmt.Println("vv =", vv)
	fmt.Println("jj =", jj)
}
```
