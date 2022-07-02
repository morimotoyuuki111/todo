# main.go

```go
package main

import (
	"github.com/gin-gonic/gin"
	"github.com/jinzhu/gorm"

	_ "github.com/mattn/go-sqlite3"
)

type Todo struct {
		gorm.Model
		Text  string
		Status string
}
```

```go
//DB初期化
func dbInit(){
		db, err :=gorm.Open("sqlite3","test.sqlite3")
		if err != nil {
				panic("データベース開けず！(dbInit)")
		}
		db.AutoMigrate(&Todo{})
		defer db.Close()
}
```

```go
//DB追加
func dbInsert(text string, status string) {
		db, err :=gorm.Open("sqlite3","test.sqlite3")
		if err !=nil {
				panic("データベース開けず！(dbInsert)")
		}
		db.Create(&Todo{Text: text,Status: status})
		defer db.Close()
}
```
```go
//DB更新
func dbUpdate(id int, text string, status string) {
	db, err :=gorm.Open("sqlite3", "test.sqlite3")
	if err != nil {
		panic("データベース開けず!(dbUpdate)")
	}
	var todo Todo
	db.First(&todo, id)
	todo.Text = text
	todo.Status = status
	db.Save(&todo)
	db.Close()

}
```
```go
//DB削除
func dbDelete(id int){
		db,err := gorm.Open("sqlite3","test.sqlite3")
		if err !=nil {
				panic("データベース開けず!(dbDelete)")
		}
		var todo Todo
		db.First(&todo, id)
		db.Delete(&todo)
		db.Close()
}
```

```go
//DB全取得
func dbGetAll() []Todo {
	db, err := gorm.Open("sqlite3", "test.sqlite3")
	if err != nil {
			panic("データベース開けず！(dbGetAll())")
	}
	var todos []Todo
	db.Order("created_at desc").Find(&todos)
	db.Close()
	return todos
}
```
```go
//DB一つ取得
func dbGetOne(id int) Todo {
		db, err := gorm.Open("sqlite3","test.sqlite3")
		if err != nil {
			panic("データベース開けず!(dbGetOne())")
		}
		var todo Todo
		db.First(&todo, id)
		db.Close()
		return todo
}
```

```go
func main() {
    router := gin.Default()
    router.LoadHTMLGlob("templates/*.html")

    router.GET("/", func(ctx *gin.Context){
        ctx.HTML(200, "index.html", gin.H{})
    })

    router.Run()
}
```
