+++
title = "Golang 教程"
description = ""
weight = 2
+++

## 教程：

```bash
mkdir hello
cd hello

go mod init example/hello
vim hello.go
```

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
}
```

```bash
go run .
```

引用外部包。

```go
package main

import "fmt"

import "rsc.io/quote"

func main() {
    fmt.Println(quote.Go())
}
```

下载外部包，并在 `go.mod` 中添加依赖以及生成认证文件 `go.sum`。

```bash
go mod tidy
```

## 教程：创建一个 Go 模块 

### 创建模块

```bash
mkdir greetings
cd greetings/

go mod init example.com/greetings

vim greetings.go
```

```go
package greetings

import "fmt"

// Hello returns a greeting for the named person
func Hello(name string) string {
    // Return a greeting that embeds the the in a message.
    message := fmt.Sprintf("Hi, %v. Welcome!", name)
    return message
}
```

### 调用其他模块

此时目录结构为：

```
.
├── greetings
└── hello
```

```bash
cd hello
rm -rf *

go mod init example.com/hello

vim hello.go
```

```go

```
