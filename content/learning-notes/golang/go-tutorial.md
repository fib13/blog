+++
title = "Golang 教程"
description = ""
weight = 2
+++

## 教程：开始

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
package main

import (
	"fmt"

	"example.com/greetings"
)

func main() {
	// Get a greeting message and print it.
	message := greetings.Hello("Gladys")
	fmt.Println(message)
}
```

重定向 Go 模块路径并添加依赖：

```bash
go mod edit -replace example.com/greetings=../greetings

go mod tidy
```

运行：

```bash
go run .
```

### 错误处理

`greetings.go`：

```go
package greetings

import (
	"errors"
	"fmt"
)

// Hello returns a greeting for the named person
func Hello(name string) (string, error) {
	// If no name was given, return an error with a message.
	if name == "" {
		return "", errors.New("empty name")
	}

	// If a name was received, Return a greeting that embeds the the
	// in a message.
	message := fmt.Sprintf("Hi, %v. Welcome!", name)
	return message, nil
}
```

`hello.go`：

```go
package main

import (
	"fmt"
	"log"

	"example.com/greetings"
)

func main() {
	// Set properties of the predefined Logger, including the log
	// entry prefix and a flag to disable printing the time, source
	// file, and line number.
	log.SetPrefix("greetings: ")
	log.SetFlags(0)

	// Request a greeting message.
	message, err := greetings.Hello("")
	// If an error was returned, print it to the console and exit the program.
	if err != nil {
		log.Fatal(err)
	}

	// If no error was returned, print the returned message to the massage.
	fmt.Println(message)
}
```

### 随机问候

`greetings.go`:

```go
package greetings

import (
	"errors"
	"fmt"
	"math/rand"
	"time"
)

// Hello returns a greeting for the named person
func Hello(name string) (string, error) {
	// If no name was given, return an error with a message.
	if name == "" {
		return "", errors.New("empty name")
	}

	// If a name was received, Return a greeting that embeds the the in a
	// message.
	message := fmt.Sprintf(randomFormat(), name)
	return message, nil
}

// init sets initial values for variables used in the fuction.
func init() {
	rand.Seed(time.Now().UnixNano())
}

// randomFormat returns one of a set of greeting messages. The returned message
// is selected at random.
func randomFormat() string {
	// A slice of message formats.
	formats := []string{
		"Hi, %v. Welcome!",
		"Great to see you, %v!",
		"Hail, %v! Well met!",
	}

	// Return a randomly selected message format by specifying a random index
	// for the slice of formats.
	return formats[rand.Intn(len(formats))]
}
```

`hello.go`：

```go
message, err := greetings.Hello("Gladys")
```

### 多人问候

`greetings.go`：

```go
package greetings

import (
	"errors"
	"fmt"
	"math/rand"
	"time"
)

// Hello returns a greeting for the named person
func Hello(name string) (string, error) {
	// If no name was given, return an error with a message.
	if name == "" {
		return "", errors.New("empty name")
	}

	// If a name was received, Return a greeting that embeds the the in a
	// message.
	message := fmt.Sprintf(randomFormat(), name)
	return message, nil
}

// Hellos returns a map that associates each of the named people with a greeting
// message.
func Hellos(names []string) (map[string]string, error) {
	// A map to associate names with messages.
	messages := make(map[string]string)
	// Loop through the received slice of names, calling the Hello function to
	// get a message for each name.
	for _, name := range names {
		message, err := Hello(name)
		if err != nil {
			return nil, err
		}
		// In the map, associate the retrieved message with the name.
		messages[name] = message
	}
	return messages, nil
}
```

`hello.go`

```go
func main() {
	// Set properties of the predefined Logger, including the log
	// entry prefix and a flag to disable printing the time, source
	// file, and line number.
	log.SetPrefix("greetings: ")
	log.SetFlags(0)

	// A slice of names.
	names := []string{"Gladys", "Samantha", "Darrin"}

	// Request greeting messages for the names.
	messages, err := greetings.Hellos(names)
	// If an error was returned, print it to the console and exit the program.
	if err != nil {
		log.Fatal(err)
	}

	// If no error was returned, print the returned message to the massage.
	fmt.Println(messages)
}
```

### 测试

`greetings_test.go`：

```go
package greetings

import (
	"regexp"
	"testing"
)

// TestHelloName calls greetings.Hello with a name, cheking for a vaild return
// value.
func TestHelloName(t *testing.T) {
	name := "Gladys"
	want := regexp.MustCompile(`\b` + name + `\b`)
	msg, err := Hello("Gladys")
	if !want.MatchString(msg) || err != nil {
		t.Fatalf(`Hello("Gladys") = %q, %v, want match for %#q, nil`,
			msg, err, want)
	}
}

// TestHelloEmpty calls greetings.Hello with an empty string, checking for an
// error.
func TestHelloEmpty(t *testing.T) {
	msg, err := Hello("")
	if msg != "" || err == nil {
		t.Fatalf(`Hello("") = %q, %v, want "", error`, msg, err)
	}
}
```

### 编译安装

编译：

```bash
go build

./hello
```

安装：

```bash
# 显示安装目录
go list -f '{{ .Target }}'

# 配置环境变量
export PATH=$PATH:/path/to/your/install/directory
# 安装
go install

# 运行
hello
```







