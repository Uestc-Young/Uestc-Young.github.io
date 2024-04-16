---
title: Go语言入门
date: 2023-07-29 16:46:28
tags: [Language learning]
categories: [Go]
mathjax: true
typora-root-url: ..
---

## Go语言入门

### 第一个hello world

```go
package main

import "fmt"

func main() {
	fmt.Println("hello,world!")
}
```

注意`go`语言中`{}`不能单独在一行写出，例如

```go
func main()
{}
```

是不允许的。
同时注意，我们导入了`fmt`这个包，但是像`fmt.Printf`这种形式的变量才能够被外部访问：
**只有标识符为大写字母开头的才能被外部变量引用`(like public)`，而小写字母开头的标识符对外是不可见的`(like private)`。**

<!--more-->

### 基础语法

```go
package main

import "fmt"

func main() {
	var x int = 123
	var y string = "114-514"
	var z string = "Code=%dDate=%s"
	var url = fmt.Sprintf(z, x, y)
	fmt.Println(url)
	fmt.Printf(z, x, y)
}
```

两种输出方法，使用`Sprintf`的时候是将值赋给一个变量再使用`fmt`进行输出的。

### 数据类型

还是和C之类的差不多的数据类型，不做赘述。
常规方式

```go
var a int = 123
var b bool = false
var c = "xv33233"
```

使用`:=`声明变量

```go
xv := 123
fmt.Println(xv)
```

### 条件与循环语句

`if`条件

```go
func main() {
	xv := 123
	if xv < 100 {
		fmt.Println("Less than 100!")
		fmt.Printf("The value of xv is :%d", xv)
	} else {
		fmt.Println("Bigger than 100!")
		fmt.Printf("The value of xv is :%d", xv)
	}
}
```

`switch`和`C`同理

`for`循环

```go
func main() {
	sum := 0
	for i := 0; i <= 10; i++ {
		sum += i
	}
	fmt.Printf("The value of sum : %d", sum)
}

func main() {
	numbers := [5]int{1, 2, 3, 4, 5}
	for i := range numbers {
		fmt.Println(numbers[i])
	}
}
```

### 函数

```go
func max(x int, y int) int {
	if x > y {
		return x
	} else {
		return y
	}
}
```

### 指针

注意和`C`语言不同的地方在于，不能使用`&array_name`来访问数组首元素的地址。

```go
var p *int 
```

空指针：`nil`

### 结构体

```go
type Books struct {
	title  string
	author string
	isbn   int
}

func main() {
	book1 := Books{"DataBase", "XV33233", 114514}
	fmt.Println(book1.title)
}
```

### Go的特性

`range`来遍历数组或者切片或`map`

```go
func main() {
	numbers := [5]int{1, 2, 3, 4, 5}
	for index, value := range numbers {
		fmt.Printf("The %d index number is %d\n", index, value)
	}
}
```

可以使用`append()`函数来给切片增加内容

`map(集合)`
语法规则：`map[indextype]valuetype {}`

```go
m := map[int]int{
    0: 1,
    1: 2,
    2: 3,
}
```

`interface(接口)`

```go
type Phone interface {
	call()
}

type Nokia struct {
}

func (nokia_phone Nokia) call() {
	fmt.Println("Nokia calling!")
}

func main() {
	phone := new(Nokia)
	phone.call()
}
```

`error(错误处理)`

```go
import (
	"errors"
	"fmt"
)

func main() {
	fmt.Println(Divide(12.0, 0))
}

func Divide(x float64, y float64) (float64, error) {
	if y == 0 {
		return 0, errors.New("Error Input!")
	} else {
		result := x / y
		return result, nil
	}
}
```
