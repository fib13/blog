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

## 教程：访问关系型数据库

### 准备

安装 MySQL：

```bash
docker run -itd --name mysql -e MYSQL_ROOT_PASSWORD=123456 -p 3306:3306 mysql:5.7 --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
```

```bash
mkdir data-access

cd data-access

go mod init example/data-access
```

### 初始化数据库

```bash
docker exec -ti mysql mysql -u root -p123456
```

创建数据库：

```sql
create database recordings;
```

`create-tables.sql`：

```sql
DROP TABLE IF EXISTS album;
CREATE TABLE album (
	id			  INT AUTO_INCREMENT NOT NULL,
	title		  VARCHAR(128) NOT NULL,
	artist	  VARCHAR(128) NOT NULL,
	price		  DECIMAL(5,2) NOT NULL,
	PRIMARY KEY (`id`)
);

INSERT INTO album
  (title, artist, price)
VALUES
  ('Blue Train', 'John Coltrane', 56.99),
  ('Giant Steps', 'John Coltrane', 63.99),
  ('Jeru', 'Gerry Mulligan', 17.99),
  ('Sarah Vaughan', 'Sarah Vaughan', 34.98);
```

初始化数据：

```bash
docker exec -ti mysql mysql -uroot -p123456 recordings -e "source /tmp/create-tables.sql
```

```
mysql> select * from album;
+----+---------------+----------------+-------+
| id | title         | artist         | price |
+----+---------------+----------------+-------+
|  1 | Blue Train    | John Coltrane  | 56.99 |
|  2 | Giant Steps   | John Coltrane  | 63.99 |
|  3 | Jeru          | Gerry Mulligan | 17.99 |
|  4 | Sarah Vaughan | Sarah Vaughan  | 34.98 |
+----+---------------+----------------+-------+
```

### 获取数据库句柄并链接

`main.go`

```go
package main

import (
	"database/sql"
	"fmt"
	"log"
	"os"

	"github.com/go-sql-driver/mysql"
)

var db *sql.DB

func main() {
	// Capture connection properties.
	cfg := mysql.Config{
		User:                 os.Getenv("DBUSER"),
		Passwd:               os.Getenv("DBPASS"),
		Net:                  "tcp",
		Addr:                 "127.0.0.1:3306",
		DBName:               "recordings",
		AllowNativePasswords: true,
	}
	// Get a database handle.
	var err error
	db, err = sql.Open("mysql", cfg.FormatDSN())
	if err != nil {
		log.Fatal(err)
	}

	pingErr := db.Ping()
	if pingErr != nil {
		log.Fatal(pingErr)
	}
	fmt.Println("Connected!")
}
```

- `sql.DB` 表示一个连接池。
- `sql.Open` 的第一个参数是驱动名称。
- `cfg.FormatDSN()` 将配置转换为 "User:Passwd@tcp(127.0.0.1:3306)/recordings"。
- db.Ping() 检查数据库连通性。

```bash
go get .

go run .
```

### 查询多行

```go
package main

import (
	"database/sql"
	"fmt"
	"log"
	"os"

	"github.com/go-sql-driver/mysql"
)

var db *sql.DB

type Album struct {
	ID     int64
	Title  string
	Artist string
	Price  float32
}

func main() {
	// Capture connection properties.
	cfg := mysql.Config{
		User:                 os.Getenv("DBUSER"),
		Passwd:               os.Getenv("DBPASS"),
		Net:                  "tcp",
		Addr:                 "127.0.0.1:3306",
		DBName:               "recordings",
		AllowNativePasswords: true,
	}
	// Get a database handle.
	var err error
	db, err = sql.Open("mysql", cfg.FormatDSN())
	if err != nil {
		log.Fatal(err)
	}

	pingErr := db.Ping()
	if pingErr != nil {
		log.Fatal(pingErr)
	}
	fmt.Println("Connected!")

	albums, err := albumByArtist("John Coltrane")
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("Albums found: %v\n", albums)
}

// albumByArtist queries for album that have the specified artist name.
func albumByArtist(name string) ([]Album, error) {
	// An album slice to hold data from returned rows.
	var albums []Album

	rows, err := db.Query("SELECT * FROM album WHERE artist = ?", name)
	if err != nil {
		return nil, fmt.Errorf("albumByArtist %q: %v", name, err)
	}
	defer rows.Close()
	// Loop through rows, using Scan to assign column data to struct fields.
	for rows.Next() {
		var alb Album
		if err := rows.Scan(&alb.ID, &alb.Title, &alb.Artist, &alb.Price); err != nil {
			return nil, fmt.Errorf("albumByArtist %q: %v", name, err)
		}
		albums = append(albums, alb)
	}
	if err := rows.Err(); err != nil {
		return nil, fmt.Errorf("albumByArtist %q: %v", name, err)
	}
	return albums, nil
}
```

- `Query` 方法返回一个 `*Rows` 的指针，存放结果集。
- `defer rows.Close()` 在函数退出阶段调用，防止连接泄漏。
- `rows.Scan` 扫描当前行的列值分配给 `Album` 结构体。

### 查询单行

`albumByID` 函数：

```go
// albumByID queries for the album with the specified ID.
func albumByID(id int64) (Album, error) {
	// An album to hold data from the returned row.
	var alb Album

	row := db.QueryRow("SELECT * FROM album WHERE id = ?", id)
	if err := row.Scan(&alb.ID, &alb.Title, &alb.Artist, &alb.Price); err != nil {
		if err == sql.ErrNoRows {
			return alb, fmt.Errorf("albumByID %d: no such album", id)
		}
		return alb, fmt.Errorf("albumByID %d: %v", id, err)
	}
	return alb, nil
}
```

`main` 函数添加：

```go
	// Hard-code ID 2 here to test the query.
	alb, err := albumByID(2)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("Album found: %v\n", alb)
```

## 添加数据

`addAlbum` 函数：

```go
// addAlbum adds the specified album to the database, returning the album ID of
// the nuw entry
func addAlbum(alb Album) (int64, error) {
	result, err := db.Exec("INSERT INTO album (title, artist, price) VALUES (?, ?, ?)", alb.Title, alb.Artist, alb.Price)
	if err != nil {
		return 0, fmt.Errorf("addAlbum: %v", err)
	}
	id, err := result.LastInsertId()
	if err != nil {
		return 0, fmt.Errorf("addAlbum: %v", err)
	}
	return id, nil
}
```

- `db.Exec` 方法执行不返回数据的 SQL 语句。
- `Result.LastInsertId` 方法返回插入数据库行的 ID。

`main` 函数添加：

```go
	albID, err := addAlbum(Album{
		Title:  "The Modern Sound of Betty Carter",
		Artist: "Betty Carter",
		Price:  49.99,
	})
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("ID of added album: %v\n", albID)
```

## 教程：使用 Gin 开发 RESTful API

### 创建数据

```go
package main

// album represents data about a record album.
type album struct {
	ID     string  `json:"id"`
	Title  string  `json:"title"`
	Artist string  `json:"artist"`
	Price  float64 `json:"price"`
}

// albums slice to seed record album data.
var albums = []album{
	{ID: "1", Title: "Blue Train", Artist: "John Coltrane", Price: 56.99},
	{ID: "2", Title: "Jeru", Artist: "Gerry Mulligan", Price: 17.99},
	{ID: "3", Title: "Sarah Vaughan and Clifford Brown", Artist: "Sarah Vaughan", Price: 39.99},
}
```

### 返回所有数据

```go
import (
	"net/http"

	"github.com/gin-gonic/gin"
)

// ...

func main() {

	router := gin.Default()
	router.GET("/albums", getAlbums)

	router.Run("localhost:8080")
}

// getAlbums responds with the list of all albums as JSON.
func getAlbums(c *gin.Context) {
	c.IndentedJSON(http.StatusOK, albums)
}
```

```bash
go get .

go run .

curl http://localhost:8080/albums
```

### 添加新项

添加 `postAlbums` 函数：

```go
// postAlbums adds an album from JSON received in the request body.
func postAlbums(c *gin.Context) {
	var newAlbum album

	// Call BindJSON to bind the received JSON to new Album.
	if err := c.BindJSON(&newAlbum); err != nil {
		return
	}

	// Add the new album to the slice.
	albums = append(albums, newAlbum)
	c.IndentedJSON(http.StatusCreated, newAlbum)
}
```

`main` 函数添加：

```go
router.POST("/albums", postAlbums)
```

```bash
curl http://localhost:8080/albums \
	--include \
	--header "Content-Type: application/json" \
	--request "POST" \
	--data '{"id": "4","title": "The Modern Sound of Betty Carter","artist": "Betty Carter","price": 49.99}'

curl http://localhost:8080/albums \
	--header "Content-Type: application/json" \
	--request "GET"
```

### 返回特定项

`getAlbumByID` 函数：

```go
// getAlbumByID locates the album whose ID value matches the id parameter sent
// by the client, then returns that album as a response.
func getAlbumByID(c *gin.Context) {
	id := c.Param("id")

	// Loop over the list of albums, looking for an album whose ID value matches
	// the parameter.
	for _, a := range albums {
		if a.ID == id {
			c.IndentedJSON(http.StatusOK, a)
			return
		}
	}
	c.IndentedJSON(http.StatusNotFound, gin.H{"message": "album not found"})
}
```

`main` 添加：

```go
	router.GET("/albums/:id", getAlbumByID)
```

- `:id` Gin 中路径前面有冒号表示该项是路径参数。

```bash
curl http://localhost:8080/albums/2

curl http://localhost:8080/albums/5
```

### 完整代码

```go
package main

import (
	"net/http"

	"github.com/gin-gonic/gin"
)

// album represents data about a record album.
type album struct {
	ID     string  `json:"id"`
	Title  string  `json:"title"`
	Artist string  `json:"artist"`
	Price  float64 `json:"price"`
}

// albums slice to seed record album data.
var albums = []album{
	{ID: "1", Title: "Blue Train", Artist: "John Coltrane", Price: 56.99},
	{ID: "2", Title: "Jeru", Artist: "Gerry Mulligan", Price: 17.99},
	{ID: "3", Title: "Sarah Vaughan and Clifford Brown", Artist: "Sarah Vaughan", Price: 39.99},
}

func main() {

	router := gin.Default()
	router.GET("/albums", getAlbums)
	router.GET("/albums/:id", getAlbumByID)
	router.POST("/albums", postAlbums)

	router.Run("localhost:8080")
}

// getAlbums responds with the list of all albums as JSON.
func getAlbums(c *gin.Context) {
	c.IndentedJSON(http.StatusOK, albums)
}

// postAlbums adds an album from JSON received in the request body.
func postAlbums(c *gin.Context) {
	var newAlbum album

	// Call BindJSON to bind the received JSON to new Album.
	if err := c.BindJSON(&newAlbum); err != nil {
		return
	}

	// Add the new album to the slice.
	albums = append(albums, newAlbum)
	c.IndentedJSON(http.StatusCreated, newAlbum)
}

// getAlbumByID locates the album whose ID value matches the id parameter sent
// by the client, then returns that album as a response.
func getAlbumByID(c *gin.Context) {
	id := c.Param("id")

	// Loop over the list of albums, looking for an album whose ID value matches
	// the parameter.
	for _, a := range albums {
		if a.ID == id {
			c.IndentedJSON(http.StatusOK, a)
			return
		}
	}
	c.IndentedJSON(http.StatusNotFound, gin.H{"message": "album not found"})
}
```