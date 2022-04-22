# 1 GO基础

## 1.1 基础环境

+ 安装包下载地址为： https://golang.google.cn/dl/
+ 设置env变量

```shell
WORK_DIR=$(cd $(dirname $0) && pwd )
echo $WORK_DIR

# 解决go get 超时问题
go env -w GOPROXY="https://goproxy.cn"
# 解决gopath无效的问题，不使用go.mod
go env -w GO111MODULE="auto"
go env -w GOPATH="${WORK_DIR}/go_lib"

```

> **GO111MODULE**
>
> + GO111MODULE=off，无模块支持，go命令行将不会支持module功能，寻找依赖包的方式将会沿用旧版本那种通过vendor目录或者GOPATH模式来查找。
> + GO111MODULE=on，模块支持，go命令行会使用modules，而一点也不会去GOPATH目录下查找。
> + GO111MODULE=auto，默认值，go命令行将会根据当前目录来决定是否启用module功能。这种情况下可以分为两种情形：
>   （1）当前目录在GOPATH/src之外且该目录包含go.mod文件，开启模块支持。
>   （2）当前文件在包含go.mod文件的目录下面

+ 编写下面的内容到hello.go，执行`go run hello.go`

```go
package main

import "fmt"

func main() {
   /* 这是我的第一个简单的程序 */
   fmt.Println("Hello, World!")
}
```

+ 字符串连接：`fmt.Println("Google" + "Runoob")`

+ 格式化字符串

​       Go 语言中使用 **fmt.Sprintf** 格式化字符串并赋值给新串：

```go
package main

import (
    "fmt"
)

func main() {
   // %d 表示整型数字，%s 表示字符串
    var stockcode=123
    var enddate="2020-12-31"
    var url="Code=%d&endDate=%s"
    var target_url=fmt.Sprintf(url,stockcode,enddate)
    fmt.Println(target_url)
}
```

```shell
[root@5d09d6b9f9d5 golang]# go run 01.go
Code=123&endDate=2020-12-31
[root@5d09d6b9f9d5 golang]#
```

## 1.2 数据类型

Go 语言按类别有以下几种数据类型：

| 序号      | 类型        | 描述                                   |
| :-------- | :---------- | :------------------------------------- |
| 1    | **布尔型** | 布尔型的值只可以是常量 true 或者 false。一个简单的例子：var b bool = true。 |
| 2    | **数字类型** | 整型 int 和浮点型 float32、float64，Go 语言支持整型和浮点型数字，并且支持复数，其中位的运算采用补码。 |
| 3    | **字符串类型:** | 字符串就是一串固定长度的字符连接起来的字符序列。Go 的字符串是由单个字节连接起来的。Go 语言的字符串的字节使用 UTF-8 编码标识 Unicode 文本。 |
| 4    | **派生类型:** | 包括：(a) 指针类型（Pointer）；(b) 数组类型；(c) 结构化类型(struct)；(d) Channel 类型；(e) 函数类型；(f) 切片类型；(g) 接口类型（interface）；(h) Map 类型； |

------

### 1.2.1  数字类型

Go 也有基于架构的类型，例如：int、uint 和 uintptr。

| 序号 | 类型 | 描述                                            |
| :--- | :------ | :----------------------------------------- |
| 1    | **uint8** |  无符号 8 位整型 (0 到 255)                         |
| 2    | **uint16**  | 无符号 16 位整型 (0 到 65535)                     |
| 3    | **uint32**  | 无符号 32 位整型 (0 到 4294967295)                |
| 4    | **uint64** |  无符号 64 位整型 (0 到 18446744073709551615)      |
| 5    | **int8**  | 有符号 8 位整型 (-128 到 127)                       |
| 6    | **int16**  | 有符号 16 位整型 (-32768 到 32767)                 |
| 7    | **int32**  | 有符号 32 位整型 (-2147483648 到 2147483647)       |
| 8    | **int64**  | 有符号 64 位整型 (-9223372036854775808 到 9223372036854775807) |

### 1.2.2  浮点型

| 序号 | 类型 | 描述                        |
| :--- | :-------------------------------- |
| 1    | **float32**  | IEEE-754 32位浮点型数 |
| 2    | **float64**  | IEEE-754 64位浮点型数 |
| 3    | **complex64**  | 32 位实数和虚数     |
| 4    | **complex128**  | 64 位实数和虚数    |

------

### 1.2.3 其他数字类型

以下列出了其他更多的数字类型：

| 序号 | 类型 | 描述                               |
| :--- | :--------------------------------------- |
| 1    | **byte**  | 类似 uint8                      |
| 2    | **rune**  | 类似 int32                      |
| 3    | **uint**  | 32 或 64 位                     |
| 4    | **int**  | 与 uint 一样大小                 |
| 5    | **uintptr**  | 无符号整型，用于存放一个指针 |

## 1.3 基础语法

### 1.3.1 变量

```go
//声明变量的一般形式是使用 var 关键字
var identifier type 

//可以一次声明多个变量
var identifier1, identifier2 type

//第一种，指定变量类型，如果没有初始化，则变量默认为零值。
var v_name v_type
v_name = value

//第二种，根据值自行判定变量类型
var v_name = value

//第三种，使用 := 声明变量并初始化
v_name := value

//类型相同多个变量, 非全局变量
var vname1, vname2, vname3 type
vname1, vname2, vname3 = v1, v2, v3
var vname1, vname2, vname3 = v1, v2, v3 // 和 python 很像,不需要显示声明类型，自动推断
vname1, vname2, vname3 := v1, v2, v3 

// 这种因式分解关键字的写法一般用于声明全局变量
var (
    vname1 v_type1
    vname2 v_type2
)
```

```go
package main
import (
                "fmt"
                "strconv"
)
func main() {
    var a string = "Runoob"
    fmt.Println(a)

    var b, c int = 1, 2
    fmt.Println(b, c)

        fmt.Println("---> "+strconv.Itoa(b))
}
```

```shell
[root@5d09d6b9f9d5 golang]# go run 02.go
Runoob
1 2
---> 1
[root@5d09d6b9f9d5 golang]#
```

### 1.3.2 常量

```go
//常量的定义格式：
const identifier [type] = value

//多个相同类型的声明可以简写为：
const c_name1, c_name2 = value1, value2

//常量还可以用作枚举
const (
    Unknown = 0
    Female = 1
    Male = 2
)
```

+ iota

> iota，特殊常量，可以认为是一个可以被编译器修改的常量。
>
> iota 在 const关键字出现时将被重置为 0(const 内部的第一行之前)，const 中每新增一行常量声明将使 iota 计数一次(iota 可理解为 const 语句块中的行索引)。

```go
const (
    a = iota
    b = iota
    c = iota
)
//第一个 iota 等于 0，每当 iota 在新的一行被使用时，它的值都会自动加 1；所以 a=0, b=1, c=2 
//可以简写为如下形式：
const (
    a = iota
    b
    c
)
```

```go
package main

import "fmt"
import "unsafe"
const (
    a = "abc"
    b = len(a)
    c = unsafe.Sizeof(a)
)

func main(){
    println(a, b, c)


       const (
            a = iota   //0
            b          //1
            c          //2
            d = "ha"   //独立值，iota += 1
            e          //"ha"   iota += 1
            f = 100    //iota +=1
            g          //100  iota +=1
            h = iota   //7,恢复计数
            i          //8
    )
    fmt.Println(a,b,c,d,e,f,g,h,i)
}

```

```shell
[root@5d09d6b9f9d5 golang]# go run 04.go
abc 3 16
0 1 2 ha ha 100 100 7 8
[root@5d09d6b9f9d5 golang]#
```

### 1.3.3 条件语句

```go
//05.go
package main

import "fmt"

func main() {
   /* 局部变量定义 */
   var a int = 100;

   /* 判断布尔表达式 */
   if a < 20 {
       /* 如果条件为 true 则执行以下语句 */
       fmt.Printf("a 小于 20\n" );
   } else {
       /* 如果条件为 false 则执行以下语句 */
       fmt.Printf("a 不小于 20\n" );
   }
   fmt.Printf("a 的值为 : %d\n", a);

}
```



```shell
[root@5d09d6b9f9d5 golang]# go run 05.go
a 不小于 20
a 的值为 : 100
[root@5d09d6b9f9d5 golang]#
```

#### switch语句

```go
package main

import "fmt"

func main() {
   /* 定义局部变量 */
   var grade string = "B"
   var marks int = 90

   switch marks {
      case 90: grade = "A"
      case 80: grade = "B"
      case 50,60,70 : grade = "C"
      default: grade = "D"  
   }

   switch {
      case grade == "A" :
         fmt.Printf("优秀!\n" )    
      case grade == "B", grade == "C" :
         fmt.Printf("良好\n" )      
      case grade == "D" :
         fmt.Printf("及格\n" )      
      case grade == "F":
         fmt.Printf("不及格\n" )
      default:
         fmt.Printf("差\n" );
   }
   fmt.Printf("你的等级是 %s\n", grade );      
}
```
> 注意：fallthrough 会强制执行后面的 case 语句，fallthrough 不会判断下一条 case 的表达式结果是否为 true

#### Type Switch(type)

```go
package main

import "fmt"

func main() {
   var x interface{}
     
   switch i := x.(type) {
      case nil:  
         fmt.Printf(" x 的类型 :%T",i)                
      case int:  
         fmt.Printf("x 是 int 型")                      
      case float64:
         fmt.Printf("x 是 float64 型")          
      case func(int) float64:
         fmt.Printf("x 是 func(int) 型")                      
      case bool, string:
         fmt.Printf("x 是 bool 或 string 型" )      
      default:
         fmt.Printf("未知型")    
   }  
}
```

#### select 语句

```go
package main

import "fmt"

func main() {
   var c1, c2, c3 chan int
   var i1, i2 int
   select {
      case i1 = <-c1:
         fmt.Printf("received ", i1, " from c1\n")
      case c2 <- i2:
         fmt.Printf("sent ", i2, " to c2\n")
      case i3, ok := (<-c3):  // same as: i3, ok := <-c3
         if ok {
            fmt.Printf("received ", i3, " from c3\n")
         } else {
            fmt.Printf("c3 is closed\n")
         }
      default:
         fmt.Printf("no communication\n")
   }    
}
```

### 1.3.4 循环语句

```go
package main

import "fmt"

func main() {
        sum := 1
        for ; sum <= 10; {
                sum += sum
        }
        fmt.Println(sum)

        // 这样写也可以，更像 While 语句形式
        for sum <= 10{
                sum += sum
        }
        fmt.Println(sum)
}
```

#### For-each range 循环

> 这种格式的循环可以对字符串、数组、切片等进行迭代输出元素。

```go
package main
import "fmt"

func main() {
        strings := []string{"google", "runoob"}
        for i, s := range strings {
                fmt.Println(i, s)
        }

        numbers := [6]int{1, 2, 3, 5}
        for i,x:= range numbers {
                fmt.Printf("第 %d 位 x 的值 = %d\n", i,x)
        }  
}
```


### 1.3.5 数组

+ 数组声明 `var balance [10] float32`

+ 数组初始化 
 `var balance = [5]float32{1000.0, 2.0, 3.4, 7.0, 50.0}`

 `balance := [5]float32{1000.0, 2.0, 3.4, 7.0, 50.0}`

+ 如果数组长度不确定，可以使用 **...** 代替数组的长度，编译器会根据元素个数自行推断数组的长度

  `balance := [...]float32{1000.0, 2.0, 3.4, 7.0, 50.0}`

  `balance := [5]float32{1:2.0,3:7.0}`

+ 示例

```go
//11.go
package main

import "fmt"

func main() {
   var i,j,k int
   // 声明数组的同时快速初始化数组
   balance := [5]float32{1000.0, 2.0, 3.4, 7.0, 50.0}

   /* 输出数组元素 */
   for i = 0; i < 5; i++ {
      fmt.Printf("balance[%d] = %f\n", i, balance[i] )
   }

   balance2 := [...]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
   /* 输出每个数组元素的值 */
   for j = 0; j < 5; j++ {
      fmt.Printf("balance2[%d] = %f\n", j, balance2[j] )
   }

   //  将索引为 1 和 3 的元素初始化
   balance3 := [5]float32{1:2.0,3:7.0}
   for k = 0; k < 5; k++ {
      fmt.Printf("balance3[%d] = %f\n", k, balance3[k] )
   }
}

```

```shell
[root@5d09d6b9f9d5 golang]# go run 11.go
balance[0] = 1000.000000
balance[1] = 2.000000
balance[2] = 3.400000
balance[3] = 7.000000
balance[4] = 50.000000
balance2[0] = 1000.000000
balance2[1] = 2.000000
balance2[2] = 3.400000
balance2[3] = 7.000000
balance2[4] = 50.000000
balance3[0] = 0.000000
balance3[1] = 2.000000
balance3[2] = 0.000000
balance3[3] = 7.000000
balance3[4] = 0.000000
[root@5d09d6b9f9d5 golang]#
```

#### 多维数组

+ 多维数组声明 `var variable_name [SIZE1][SIZE2]...[SIZEN] variable_type`

```go
//12.go
package main
import "fmt"

func main() {
   /* 数组 - 5 行 2 列*/
   var a = [5][2]int{ {0,0}, {1,2}, {2,4}, {3,6},{4,8}}
   var i, j int

   /* 输出数组元素 */
   for  i = 0; i < 5; i++ {
      for j = 0; j < 2; j++ {
         fmt.Printf("a[%d][%d] = %d\n", i,j, a[i][j] )
      }
   }

    // 创建二维数组
    sites := [2][2]string{}

    // 向二维数组添加元素
    sites[0][0] = "Google"
    sites[0][1] = "Runoob"
    sites[1][0] = "Taobao"
    sites[1][1] = "Weibo"

    // 显示结果
    fmt.Println(sites)

    // Step 1: 创建数组
    values := [][]int{}

    // Step 2: 使用 appped() 函数向空的二维数组添加两行一维数组
    row1 := []int{1, 2, 3}
    row2 := []int{4, 5, 6}
    values = append(values, row1)
    values = append(values, row2)

    // Step 3: 显示两行数据
    fmt.Println("Row 1")
    fmt.Println(values[0])
    fmt.Println("Row 2")
    fmt.Println(values[1])

    // Step 4: 访问第一个元素
    fmt.Println("第一个元素为：")
    fmt.Println(values[0][0])
}

```

```shell
[root@5d09d6b9f9d5 golang]# go run 12.go
a[0][0] = 0
a[0][1] = 0
a[1][0] = 1
a[1][1] = 2
a[2][0] = 2
a[2][1] = 4
a[3][0] = 3
a[3][1] = 6
a[4][0] = 4
a[4][1] = 8
[[Google Runoob] [Taobao Weibo]]
Row 1
[1 2 3]
Row 2
[4 5 6]
第一个元素为：
1
[root@5d09d6b9f9d5 golang]#
```

### 1.3.6 指针

```go
//13.go
package main

import "fmt"

func main() {
   var a int
   var ptr *int
   var pptr **int
   a = 3000
   /* 指针 ptr 地址 */
   ptr = &a
   /* 指向指针 ptr 地址 */
   pptr = &ptr
   /* 获取 pptr 的值 */
   fmt.Printf("变量 a = %d\n", a )
   fmt.Printf("指针变量 *ptr = %d\n", *ptr )
   fmt.Printf("指向指针的指针变量 **pptr = %d\n", **pptr)
}
```

```shell
[root@5d09d6b9f9d5 golang]# go run 13.go
变量 a = 3000
指针变量 *ptr = 3000
指向指针的指针变量 **pptr = 3000
[root@5d09d6b9f9d5 golang]#
```

### 1.3.7 结构体

#### 1.3.7.1 定义结构体

```
type struct_variable_type struct {
   member definition
   member definition
   ...
   member definition
}
```

```go
//14.go
package main

import "fmt"

type Books struct {
   title string
   author string
   subject string
   book_id int
}

func main() {

    // 创建一个新的结构体
    fmt.Println(Books{"Go 语言", "www.runoob.com", "Go 语言教程", 6495407})

    // 也可以使用 key => value 格式
    fmt.Println(Books{title: "Go 语言", author: "www.runoob.com", subject: "Go 语言教程", book_id: 6495407})

    // 忽略的字段为 0 或 空
   fmt.Println(Books{title: "Go 语言", author: "www.runoob.com"})

   var Book1 Books        /* 声明 Book1 为 Books 类型 */
   var Book2 Books        /* 声明 Book2 为 Books 类型 */

   /* book 1 描述 */
   Book1.title = "Go 语言"
   Book1.author = "www.runoob.com"
   Book1.subject = "Go 语言教程"
   Book1.book_id = 6495407

   /* book 2 描述 */
   Book2.title = "Python 教程"
   Book2.author = "www.runoob.com"
   Book2.subject = "Python 语言教程"
   Book2.book_id = 6495700

   /* 打印 Book1 信息 */
   fmt.Printf( "Book 1 title : %s\n", Book1.title)
   fmt.Printf( "Book 1 author : %s\n", Book1.author)
   fmt.Printf( "Book 1 subject : %s\n", Book1.subject)
   fmt.Printf( "Book 1 book_id : %d\n", Book1.book_id)

   /* 打印 Book2 信息 */
   fmt.Printf( "Book 2 title : %s\n", Book2.title)
   fmt.Printf( "Book 2 author : %s\n", Book2.author)
   fmt.Printf( "Book 2 subject : %s\n", Book2.subject)
   fmt.Printf( "Book 2 book_id : %d\n", Book2.book_id)

   printBook(Book1)
   printBook(Book2)

   printBookPoint(&Book1)
   printBookPoint(&Book2)

}

func printBook( book Books ) {
   fmt.Printf( "Book title : %s\n", book.title)
   fmt.Printf( "Book author : %s\n", book.author)
   fmt.Printf( "Book subject : %s\n", book.subject)
   fmt.Printf( "Book book_id : %d\n", book.book_id)
}

func printBookPoint( book *Books ) {
   fmt.Printf( "Book title : %s\n", book.title)
   fmt.Printf( "Book author : %s\n", book.author)
   fmt.Printf( "Book subject : %s\n", book.subject)
   fmt.Printf( "Book book_id : %d\n", book.book_id)
}
```

```shell
[root@5d09d6b9f9d5 golang]# go run 14.go
{Go 语言 www.runoob.com Go 语言教程 6495407}
{Go 语言 www.runoob.com Go 语言教程 6495407}
{Go 语言 www.runoob.com  0}
Book 1 title : Go 语言
Book 1 author : www.runoob.com
Book 1 subject : Go 语言教程
Book 1 book_id : 6495407
Book 2 title : Python 教程
Book 2 author : www.runoob.com
Book 2 subject : Python 语言教程
Book 2 book_id : 6495700
Book title : Go 语言
Book author : www.runoob.com
Book subject : Go 语言教程
Book book_id : 6495407
Book title : Python 教程
Book author : www.runoob.com
Book subject : Python 语言教程
Book book_id : 6495700
Book title : Go 语言
Book author : www.runoob.com
Book subject : Go 语言教程
Book book_id : 6495407
Book title : Python 教程
Book author : www.runoob.com
Book subject : Python 语言教程
Book book_id : 6495700
[root@5d09d6b9f9d5 golang]#
```

#### 1.3.7.2 结构体相同比较

```go
package main

import (
	"fmt"
	"reflect"
)

type Person struct {
	name  string
	age   int
	heigh int
}

func main() {
	a := Person{name: "Alice", age: 18}
	var b Person
	b.name = "Alice"
	b.age = 18
	fmt.Println(reflect.DeepEqual(a, b))
	fmt.Println(a, b)
	fmt.Println(a == b)   //也可以直接使用等于号

	p := new(Person)
	p.name = "Jim"
	fmt.Println(*p)
}

```

#### 1.3.7.3 结构体组合（类的嵌套）

```go
package main

import (
	"fmt"
)

type X struct {
	a int
}

func (x X) Print() {
	fmt.Println(x.a)
}

type Y struct {
	X
	a int
}
type Z struct {
	Y
	//	a int
}

func (z Z) Print() {
	fmt.Println(z.a)
}

func main() {
	x := X{a: 1}

	y := Y{
		X: x,
		a: 2,
	}

	z := Z{
		Y: y,
		//		a: 3,
	}

	fmt.Println(x.a, y.a, z.a)

	fmt.Println(z.Y.X.a, z, z.X.a, z.Y.a)
	z.Print()
	y.Print() //调用的是  X.Print
	z.Y.Print()
}

```



### 1.3.8 范围(Range)

>  range 关键字用于 for 循环中迭代数组(array)、切片(slice)、通道(channel)或集合(map)的元素。在数组和切片中它返回元素的索引和索引对应的值，在集合中返回 key-value 对。

```go
//15.go
package main
import "fmt"
func main() {
    //这是我们使用range去求一个slice的和。使用数组跟这个很类似
    nums := []int{2, 3, 4}
    sum := 0
    for _, num := range nums {
        sum += num
    }
    fmt.Println("sum:", sum)
    //在数组上使用range将传入index和值两个变量。上面那个例子我们不需要使用该元素的序号，所以我们使用空白符"_"省略了。有时侯我们确实需要知道它的索引。
    for i, num := range nums {
        if num == 3 {
            fmt.Println("index:", i)
        }
    }
    //range也可以用在map的键值对上。
    kvs := map[string]string{"a": "apple", "b": "banana"}
    for k, v := range kvs {
        fmt.Printf("%s -> %s\n", k, v)
    }
    //range也可以用来枚举Unicode字符串。第一个参数是字符的索引，第二个是字符（Unicode的值）本身。
    for i, c := range "go" {
        fmt.Println(i, c)
    }

        t,ok := kvs["a"] ;
        fmt.Println(t,ok)
}
```

```shell
[root@5d09d6b9f9d5 golang]# go run 15.go
sum: 9
index: 1
b -> banana
a -> apple
0 103
1 111
apple true
[root@5d09d6b9f9d5 golang]#
```

### 1.3.9 Map(集合)

````go
/* 声明变量，默认 map 是 nil */
var map_variable map[key_data_type]value_data_type

/* 使用 make 函数 */
map_variable := make(map[key_data_type]value_data_type)
````

> delete() 函数用于删除集合的元素, 参数为 map 和其对应的 key

```go
// 16.go
package main

import "fmt"

func main() {
    var countryCapitalMap map[string]string /*创建集合 */
    countryCapitalMap = make(map[string]string)

    /* map插入key - value对,各个国家对应的首都 */
    countryCapitalMap [ "France" ] = "巴黎"
    countryCapitalMap [ "Italy" ] = "罗马"
    countryCapitalMap [ "Japan" ] = "东京"
    countryCapitalMap [ "India " ] = "新德里"

    /*使用键输出地图值 */
    for country := range countryCapitalMap {
        fmt.Println(country, "首都是", countryCapitalMap [country])
    }

    /*查看元素在集合中是否存在 */
    capital, ok := countryCapitalMap [ "American" ] /*如果确定是真实的,则存在,否则不存在 */
    /*fmt.Println(capital) */
    /*fmt.Println(ok) */
    if (ok) {
        fmt.Println("American 的首都是", capital)
    } else {
        fmt.Println("American 的首都不存在")
    }

    /*删除元素*/ delete(countryCapitalMap, "France")
        fmt.Println("法国条目被删除")

        fmt.Println("删除元素后地图")

        /*打印地图*/
        for country := range countryCapitalMap {
                fmt.Println(country, "首都是", countryCapitalMap [ country ])
        }
}

```

```shell
[root@5d09d6b9f9d5 golang]# go run 16.go
France 首都是 巴黎
Italy 首都是 罗马
Japan 首都是 东京
India  首都是 新德里
American 的首都不存在
法国条目被删除
删除元素后地图
Japan 首都是 东京
India  首都是 新德里
Italy 首都是 罗马
[root@5d09d6b9f9d5 golang]#
```



## 1.4 切片(Slice)  

> ​       切片是对数组的抽象。数组的长度不可改变，与数组相比切片的长度是不固定的，可以追加元素，在追加时可能使切片的容量增大。

### 1.4.1 定义和初始化切片

```go
var identifier []type    //定义一个空切片
或
identifier := make([]type, length, capacity)  //其中 capacity 为可选参数

s :=[] int {1,2,3 }  //初始化切片，[] 表示是切片类型，{1,2,3} 初始化值依次是 1,2,3，其 cap=len=3。

s := arr[:] //初始化切片 s，是数组 arr 的引用。
s := arr[startIndex:endIndex] //将 arr 中从下标startIndex到endIndex-1下的元素创建为一个新的切片。
s := arr[startIndex:] //默认 endIndex 时将表示一直到arr的最后一个元素。
s := arr[:endIndex] //默认 startIndex 时将表示从 arr 的第一个元素开始。

```

### 1.4.2 len() cap() append() copy() 

> 切片是可索引的，并且可以由 len() 方法获取长度。
>
> 切片提供了计算容量的方法 cap() 可以测量切片最长可以达到多少。
>
> 向切片追加新元素的用 append 方法。
>
> 拷贝切片的用 copy 方法

```go
// 27.go
package main

import (
        "fmt"
)
func main() {

        var slice1 []int ; //
        printSlice(slice1)
        if( slice1  == nil){
                fmt.Println("--->slice1 is nil")
        }
        slice1 = append(slice1,1)
        slice1 = append(slice1,2)
        printSlice(slice1)

        slice2 := make([]int,3)
        printSlice(slice2)
        slice3 := make([]int,3,5)
        printSlice(slice3)
        slice3[0] = 1
        slice3[1] = 2
        slice3[2] = 3
        slice3 = append(slice3,4)
        slice3 = append(slice3,5)
        slice3 = append(slice3,6)
        printSlice(slice3)
        fmt.Println(slice3[1],slice3[5])

        printSlice2("slice3[:3]",slice3[:3])
        printSlice2("slice3[2:5]",slice3[2:5])
        printSlice2("slice3[3:]",slice3[3:])

        slice4 := make([]int, len(slice3)-1, len(slice3))
        copy(slice4,slice3)
        printSlice2("slice4",slice4)

}

func printSlice(x []int){
   fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x)
}

func printSlice2(msg string,x []int){
   fmt.Printf("%s ---> len=%d cap=%d slice=%v\n",msg,len(x),cap(x),x)
}


```

```shell
[root@5d09d6b9f9d5 golang]# go run 27.go
len=0 cap=0 slice=[]
--->slice1 is nil
len=2 cap=2 slice=[1 2]
len=3 cap=3 slice=[0 0 0]
len=3 cap=5 slice=[0 0 0]
len=6 cap=10 slice=[1 2 3 4 5 6]
2 6
slice3[:3] ---> len=3 cap=10 slice=[1 2 3]
slice3[2:5] ---> len=3 cap=8 slice=[3 4 5]
slice3[3:] ---> len=3 cap=7 slice=[4 5 6]
slice4 ---> len=5 cap=6 slice=[1 2 3 4 5]
[root@5d09d6b9f9d5 golang]#
```

## 1.5 函数

### 1.5.1 函数定义

> Go 语言函数定义格式如下：
> 
>    func function_name( [parameter list] ) [return_types] {
> 函数体
> }

```go
//09.go
package main

import "fmt"

func swap(x, y string) (string, string) {
   return y, x
}

func main() {
   a, b := swap("Google", "Runoob")
   fmt.Println(a, b)
}
```

```shell
[root@5d09d6b9f9d5 golang]# go run 09.go
Runoob Google
[root@5d09d6b9f9d5 golang]#
```

### 1.5.2 引用传递值

```go
//10.go
package main

import "fmt"

func main() {
   /* 定义局部变量 */
   var a int = 100
   var b int= 200

   fmt.Printf("交换前，a(%x) 的值 : %d\n", &a, a )
   fmt.Printf("交换前，b(%x) 的值 : %d\n", &b, b )

   /* 调用 swap() 函数
   * &a 指向 a 指针，a 变量的地址
   * &b 指向 b 指针，b 变量的地址
   */
   swap(&a, &b)

   fmt.Printf("交换后，a(%x) 的值 : %d\n", &a , a )
   fmt.Printf("交换后，b(%x) 的值 : %d\n", &b, b )
}

func swap(x *int, y *int) {
   var temp int
   temp = *x    /* 保存 x 地址上的值 */
   *x = *y      /* 将 y 值赋给 x */
   *y = temp    /* 将 temp 值赋给 y */
}
```

```shell
[root@5d09d6b9f9d5 golang]# go run 10.go
交换前，a(c000016050) 的值 : 100
交换前，b(c000016058) 的值 : 200
交换后，a(c000016050) 的值 : 200
交换后，b(c000016058) 的值 : 100
[root@5d09d6b9f9d5 golang]#
```

### 1.5.3 函数作为实参

```go
package main

import (
   "fmt"
   "math"
)

func main(){
   /* 声明函数变量 */
   getSquareRoot := func(x float64) float64 {
      return math.Sqrt(x)
   }

   /* 使用函数 */
   fmt.Println(getSquareRoot(9))

}
```

### 1.5.4 闭包

> Go 语言支持匿名函数，可作为闭包。匿名函数是一个"内联"语句或表达式。匿名函数的优越性在于可以直接使用函数内的变量，不必申明。
>
> 以下实例中，我们创建了函数 getSequence() ，返回另外一个函数。该函数的目的是在闭包中递增 i 变量，代码如下：

```go
package main

import "fmt"

func getSequence() func() int {
   i:=0
   return func() int {
      i+=1
     return i  
   }
}

func main(){
   /* nextNumber 为一个函数，函数 i 为 0 */
   nextNumber := getSequence()  

   /* 调用 nextNumber 函数，i 变量自增 1 并返回 */
   fmt.Println(nextNumber())
   fmt.Println(nextNumber())
   fmt.Println(nextNumber())
   
   /* 创建新的函数 nextNumber1，并查看结果 */
   nextNumber1 := getSequence()  
   fmt.Println(nextNumber1())
   fmt.Println(nextNumber1())
}
```

### 1.5.5 方法(类)

> Go 没有面向对象，而我们知道常见的 Java/C++ 等语言中，实现类的方法做法都是编译器隐式的给函数加一个 this 指针，而在 Go 里，这个 this 指针需要明确的申明出来，其实和其它 OO 语言并没有很大的区别。

```go
package main

import (
   "fmt"  
)

/* 定义结构体 */
type Circle struct {
  radius float64
}

//该 method 属于 Circle 类型对象中的方法
func (c Circle) getArea() float64 {
  //c.radius 即为 Circle 类型对象中的属性
  return 3.14 * c.radius * c.radius
}

func main() {
  var c1 Circle
  c1.radius = 10.00
  fmt.Println("圆的面积 = ", c1.getArea())
}
```

## 1.6 接口 

>Go 语言提供了另外一种数据类型即接口，它把所有的具有共性的方法定义在一起，任何其他类型只要实现了这些方法就是实现了这个接口。

```go
//17.go
package main

import (
    "fmt"
)

/* 定义接口 */
type Phone interface {
    call()
}

/* 定义结构体 */
type NokiaPhone struct {
}

func (nokiaPhone NokiaPhone) call() {
    fmt.Println("I am Nokia, I can call you!")
}

type IPhone struct {
}

func (iPhone IPhone) call() {
    fmt.Println("I am iPhone, I can call you!")
}

func main() {
    var phone Phone

    phone = new(NokiaPhone)
    phone.call()

    phone = new(IPhone)
    phone.call()

}

```

```shell
[root@5d09d6b9f9d5 golang]# go run 17.go
I am Nokia, I can call you!
I am iPhone, I can call you!
[root@5d09d6b9f9d5 golang]#
```



## 1.7 错误处理 

> Go 语言通过内置的错误接口提供了非常简单的错误处理机制。

```go
//19.1.go
package main

import (
    "fmt"
)
// 定义一个 DivideError 结构
type DivideError struct {
    dividee int
    divider int
}
// 实现 `error` 接口
func (de *DivideError) Error() string {
    strFormat := `
    Cannot proceed, the divider is zero.
    dividee: %d
    divider: 0
`
    return fmt.Sprintf(strFormat, de.dividee)
}
// 定义 `int` 类型除法运算的函数
func Divide(varDividee int, varDivider int) (result int, errorMsg string) {
    if varDivider == 0 {
            dData := DivideError{
                    dividee: varDividee,
                    divider: varDivider,
            }
            errorMsg = dData.Error()
            return
    } else {
            return varDividee / varDivider, ""
    }
}

func main() {
	// 正常情况
    if result, errorMsg := Divide(100, 10); errorMsg == "" {
            fmt.Println("100/10 = ", result)
    }
    // 当除数为零的时候会返回错误信息
    if _, errorMsg := Divide(100, 0); errorMsg != "" {
            fmt.Println("errorMsg is: ", errorMsg)
    }

}
```

```shell
[root@5d09d6b9f9d5 golang]# go run 19.1.go
100/10 =  10
errorMsg is:
    Cannot proceed, the divider is zero.
    dividee: 100
    divider: 0

[root@5d09d6b9f9d5 golang]#
```



## 1.8 并发

### 1.8.1 goroutine

> goroutine 语法格式：

```go
go 函数名( 参数列表 )
```

```go
//20.go
//协程并发
package main

import (
        "fmt"
        "time"
)

func say(s string) {
        for i := 0; i < 5; i++ {
                time.Sleep(100 * time.Millisecond)
                fmt.Println(s)
        }
}

func main() {
        go say("world")
        say("hello")
}

```

```shell
[root@5d09d6b9f9d5 golang]# go run 20.go
world
hello
hello
world
world
hello
hello
world
world
hello
[root@5d09d6b9f9d5 golang]#
```

### 1.8.2 通道（无缓冲）

> 通道（channel）是用来传递数据的一个数据结构。
>
> + 声明：`ch := make(chan int)`
>
> + 把 v 发送到通道 ch： `ch <- v `   
>
> + 从 ch 接收数据并把值赋给 v： `v := <-ch`

```go
//21.go
package main
import "fmt"

func sum(s []int, c chan int) {
        sum := 0
        for _, v := range s {
                sum += v
        }
        c <- sum // 把 sum 发送到通道 c
}

func main() {
        s := []int{7, 2, 8, -9, 4, 0}

        c := make(chan int) //声明通道
        go sum(s[:len(s)/2], c)
        go sum(s[len(s)/2:], c)
        x, y := <-c, <-c // 从通道 c 中接收

        fmt.Println(x, y, x+y)
}
```

```shell
[root@5d09d6b9f9d5 golang]# go run 21.go
-5 17 12
[root@5d09d6b9f9d5 golang]#
```

### 1.8.3 通道缓冲区

> + 声明：`ch := make(chan int, 100)`

```go
//22.go
package main

import (
        "fmt"
)

func fibonacci(n int, c chan int) {
        x, y := 0, 1
        for i := 0; i < n; i++ {
                c <- x
                x, y = y, x+y
        }
        close(c)
}

func main() {
        c := make(chan int, 10)
        go fibonacci(cap(c), c)
        // range 函数遍历每个从通道接收到的数据，因为 c 在发送完 10 个
        // 数据之后就关闭了通道，所以这里我们 range 函数在接收到 10 个数据
        // 之后就结束了。如果上面的 c 通道不关闭，那么 range 函数就不
        // 会结束，从而在接收第 11 个数据的时候就阻塞了。
        for i := range c {
                fmt.Println(i)
        }
}
```

```shell
[root@5d09d6b9f9d5 golang]# go run 22.go
0
1
1
2
3
5
8
13
21
34
[root@5d09d6b9f9d5 golang]#
```

### 1.8.4 有无缓冲区别

+ 无缓冲channel

```go
//无缓冲
package main

import (
	"fmt"
	"time"
)

func main() {
	//创建一个channel
	//ch := make(chan string, 10)
	ch := make(chan string)

	//写数据协程
	go func() {
		fmt.Println("写协程开启:", time.Now())
		ch <- "1"
		fmt.Println("写入成功:", time.Now())
	}()

	//读数据协程
	go func() {
		fmt.Println("读协程开启:", time.Now())
		time.Sleep(3 * time.Second)
		i := <-ch
		fmt.Println("读取成功:", i, time.Now())
	}()

	time.Sleep(5 * time.Second) //等待5秒,让所有协程跑完
	fmt.Println("ok")
}

```
> 有缓冲区，写入被阻塞，等读取后再完成
```
读协程开启: 2022-04-19 00:24:24.8375086 +0800 CST m=+0.010177601
写协程开启: 2022-04-19 00:24:24.8375086 +0800 CST m=+0.010177601
读取成功: 1 2022-04-19 00:24:27.9189821 +0800 CST m=+3.091651101
写入成功: 2022-04-19 00:24:27.9190704 +0800 CST m=+3.091739401
ok
```

+ 有缓冲区

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	//创建一个channel
	//ch := make(chan string, 10)
	ch := make(chan string)

	//写数据协程
	go func() {
		fmt.Println("写协程开启:", time.Now())
		ch <- "1"
		fmt.Println("写入成功:", time.Now())
	}()

	//读数据协程
	go func() {
		fmt.Println("读协程开启:", time.Now())
		time.Sleep(3 * time.Second)
		i := <-ch
		fmt.Println("读取成功:", i, time.Now())
	}()

	time.Sleep(5 * time.Second) //等待5秒,让所有协程跑完
	fmt.Println("ok")
}

```

> 有缓冲区，写入不会被阻塞

```
读协程开启: 2022-04-19 00:21:38.597473 +0800 CST m=+0.009406201
写协程开启: 2022-04-19 00:21:38.597473 +0800 CST m=+0.009406201
写入成功: 2022-04-19 00:21:38.6706444 +0800 CST m=+0.082577601
读取成功: 1 2022-04-19 00:21:41.6707801 +0800 CST m=+3.082713301
ok
```







### 1.8.5 遍历通道与关闭通道

> 通过 range 关键字来实现遍历读取到的数据：`v, ok := <-ch`

```go
package main

import (
        "fmt"
)

func fibonacci(n int, c chan int) {
        x, y := 0, 1
        for i := 0; i < n; i++ {
                c <- x
                x, y = y, x+y
        }
        close(c)
}

func main() {
        c := make(chan int, 10)
        go fibonacci(cap(c), c)
        // range 函数遍历每个从通道接收到的数据，因为 c 在发送完 10 个
        // 数据之后就关闭了通道，所以这里我们 range 函数在接收到 10 个数据
        // 之后就结束了。如果上面的 c 通道不关闭，那么 range 函数就不
        // 会结束，从而在接收第 11 个数据的时候就阻塞了。
        for i := range c {
                fmt.Println(i)
        }
}
```

### 1.8.6 主线程等待协程完成

#### 1.8.6.1 sync.WaitGroup

```go
package main

import (
	"fmt"
	"sync"
)

var wg sync.WaitGroup   //定义WaitGroup

func running(i int) {
    defer wg.Done()     //完成一个goruntime，执行一次Done()方法
	fmt.Println(i)
}
func main() {

	for i := 0; i < 10; i++ {
		wg.Add(1)		 //新建一个goruntime，执行一次Add()方法
		go running(i)
	}
	wg.Wait()            //在主线程中执行Wait()方法
	fmt.Println("Main End")
}

```

#### 1.8.6.2 通过channel控制

```go
package main

import (
	"fmt"
)

func running(i int, ch chan int) {
	fmt.Println(i)
	ch <- i
}
func main() {

	ch := make(chan int)
	for i := 0; i < 10; i++ {
		go running(i, ch)
		<-ch
	}
	fmt.Println("Main End")
}

```







## 1.9 命令行参数

### 1.9.1 os.Args

```go
//23.go
package main

import (
    "fmt"
    "os"
)

func main() {
    fmt.Println("命令行的参数有", len(os.Args))
    // 遍历 os.Args 切片，就可以得到所有的命令行输入参数值
    for i, v := range os.Args {
        fmt.Printf("args[%v]=%v\n", i, v)
    }
}
```

```shell
[root@5d09d6b9f9d5 golang]# go run 23.go 123 45
命令行的参数有 3
args[0]=/tmp/go-build417455773/b001/exe/23
args[1]=123
args[2]=45
[root@5d09d6b9f9d5 golang]#
```

### 1.9.2 flag包读取 

```go
//24.go
//命令行参数： XXXXX -u root -p 123456 -P 3370 -h 127.0.0.1
package main

import (
    "flag"
    "fmt"
)

func main() {
    // 定义几个变量，用于接收命令行的参数值
    var user        string
    var password    string
    var host        string
    var port        int
    // &user 就是接收命令行中输入 -u 后面的参数值，其他同理
    flag.StringVar(&user, "u", "root", "账号，默认为root")
    flag.StringVar(&password, "p", "", "密码，默认为空")
    flag.StringVar(&host, "h", "localhost", "主机名，默认为localhost")
    flag.IntVar(&port, "P", 3306, "端口号，默认为3306")

    // 解析命令行参数写入注册的flag里
    flag.Parse()
    // 输出结果
    fmt.Printf("user：%v\npassword：%v\nhost：%v\nport：%v\n",
        user, password, host, port)
}
```

```shell
[root@5d09d6b9f9d5 golang]# go run 24.go -u root -p 123456 -P 3370 -h 127.0.0.1
user：root
password：123456
host：127.0.0.1
port：3370
[root@5d09d6b9f9d5 golang]#
```

## 1.20 链表list

list 的初始化有两种方法：分别是使用 New() 函数和 var 关键字声明，两种方法的初始化效果都是一致的。

1) 通过 container/list 包的 New() 函数初始化 list      `变量名 := list.New()`

2) 通过 var 关键字声明初始化 list         `var 变量名 list.List`

| 方  法                                                | 功  能                                            |
| ----------------------------------------------------- | ------------------------------------------------- |
| InsertAfter(v interface {}, mark * Element) * Element | 在 mark 点之后插入元素，mark 点由其他插入函数提供 |
| InsertBefore(v interface {}, mark * Element) *Element | 在 mark 点之前插入元素，mark 点由其他插入函数提供 |
| PushBackList(other *List)                             | 添加 other 列表元素到尾部                         |
| PushFrontList(other *List)                            | 添加 other 列表元素到头部                         |



```go
package main

import (
	"container/list"
	"fmt"
)

type Person struct {
	name string
	age  int
}

func main() {
	l := list.New()
	// 尾部添加
	l.PushBack("canon")
	// 头部添加
	l.PushFront(67)
	l.PushFront(Person{"Alime", 11})
	for i := l.Front(); i != nil; i = i.Next() {
		fmt.Println(i.Value)
	}
	fmt.Println("=================")
	fmt.Println(l.Front().Value)

	var alime Person = l.Front().Value.(Person)
	fmt.Println(alime.name)
}

```

```
{Alime 11}
67
canon
=================
{Alime 11}
Alime
```



# 2 应用使用

## 2.1 数据库访问

### 2.1.1 Mysql

```go
/*
CREATE TABLE `person` (
    `user_id` int(11) NOT NULL AUTO_INCREMENT,
    `username` varchar(260) DEFAULT NULL,
    `sex` varchar(260) DEFAULT NULL,
    `email` varchar(260) DEFAULT NULL,
    PRIMARY KEY (`user_id`)
  ) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;

CREATE TABLE place (
    country varchar(200),
    city varchar(200),
    telcode int
)ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;

insert into person(username, sex, email)values("stu001", "man", "stu01@qq.com") ;
*/

package main

import (
    "fmt"
    _ "github.com/go-sql-driver/mysql"
    "github.com/jmoiron/sqlx"
        "encoding/json"
)

type Person struct {
    UserId   int    `db:"user_id"`
    Username string `db:"username"`
    Sex      string `db:"sex"`
    Email    string `db:"email"`
}

type Place struct {
    Country string `db:"country"`
    City    string `db:"city"`
    TelCode int    `db:"telcode"`
}


func conn_mysql(username string, password string, host string, port string, tablespace string) *sqlx.DB {

        //database, err := sqlx.Open("mysql", "root:Sq_123456@tcp(127.0.0.1:3306)/sq_iptables")
    database, err := sqlx.Open("mysql", ""+username+":"+password+"@tcp("+host+":"+port+")/"+tablespace+"")
    if err != nil {
        fmt.Println("open mysql failed,", err)
        return nil
    }
    //defer database.Close()
    return database
    //defer Db.Close()  // 注意这行代码要写在上面err判断的下面
}

func main() {

        var Db =  conn_mysql("root","123456","127.0.0.1","3306","MobileOA")
        defer Db.Close()
    var person []Person
    //err := Db.Select(&person, "select user_id, username, sex, email from person where user_id = ?", 2)
    err := Db.Select(&person, "select user_id, username, sex, email from person")
    if err != nil {
        fmt.Println("exec failed, ", err)
        return
    }
    fmt.Println("select succ:", person)

        //转成json
        jsonBytes, err := json.Marshal(person)
        if err != nil {
                fmt.Println(err)
        }
        fmt.Println(string(jsonBytes))
}
```

+ mysql_Transaction.go

```go
//mysql_Transaction.go
package main

    import (
        "fmt"

        _ "github.com/go-sql-driver/mysql"
        "github.com/jmoiron/sqlx"
    )

    type Person struct {
        UserId   int    `db:"user_id"`
        Username string `db:"username"`
        Sex      string `db:"sex"`
        Email    string `db:"email"`
    }

    type Place struct {
        Country string `db:"country"`
        City    string `db:"city"`
        TelCode int    `db:"telcode"`
    }

    var Db *sqlx.DB

    func init() {
        database, err := sqlx.Open("mysql", "root:123456@tcp(127.0.0.1:3306)/MobileOA")
        if err != nil {
            fmt.Println("open mysql failed,", err)
            return
        }
        Db = database
    }

    func main() {
        conn, err := Db.Begin()
        if err != nil {
            fmt.Println("begin failed :", err)
            return
        }

        r, err := conn.Exec("insert into person(username, sex, email)values(?, ?, ?)", "stu001", "man", "stu01@qq.com")
        if err != nil {
            fmt.Println("exec failed, ", err)
            conn.Rollback()
            return
        }
        id, err := r.LastInsertId()
        if err != nil {
            fmt.Println("exec failed, ", err)
            conn.Rollback()
            return
        }
        fmt.Println("insert succ:", id)

        r, err = conn.Exec("insert into person(username, sex, email)values(?, ?, ?)", "stu001", "man", "stu01@qq.com")
        if err != nil {
            fmt.Println("exec failed, ", err)
            conn.Rollback()
            return
        }
        id, err = r.LastInsertId()
        if err != nil {
            fmt.Println("exec failed, ", err)
            conn.Rollback()
            return
        }
        fmt.Println("insert succ:", id)

        conn.Commit()
    }

```

### 2.1.2 Postgres

```go
package main

import (
        //"database/sql" //通用的接口
        "github.com/jmoiron/sqlx"
        "fmt"
        _ "github.com/bmizerany/pq" //必须要有相应的驱动
)

const (
        host     = "localhost"
        port     = 5432
        user     = "postgres"
        password = ""                   //你自己数据库的密码
        dbname   = "genericdb" //创建的数据库
)


var db *sqlx.DB // 连接池对象
var err error

type product struct {
        ProductNo string
        Name      string
        Price     float64
}

type productDao struct{}


func main() {
        pd := new(productDao)
        err = pd.initDB()
        if err != nil {
                fmt.Println("initDB() failed. ")
        }
        defer pd.closeDB()
        pd.doQueryAll()
        //pd.doPreQueryByName("apple")
}

func (pd *productDao) initDB() (err error) {
        pdqlInfo := fmt.Sprintf("host=%s port=%d user=%s "+
                "password=%s dbname=%s sslmode=disable",
                host, port, user, password, dbname)
        //  pdqlInfo := "postgres:密码@tcp(127.0.0.1:5432)/test" // 用户名:密码@tcp(ip端口)/数据库名字，暂时出错
        db, err = sqlx.Open("postgres", pdqlInfo) //Open(driverName 驱动名字, dataSourceName string 数据库信息)
        // DB 代表一个具有零到多个底层连接的连接池，可以安全的被多个go程序同时使用
        //这里的open函数只是验证参数是否合法，而不会创建和数据库的连接,也不会检查账号密码是否正确
        if err != nil {
                fmt.Println("Wrong args.Connected failed.")
                return err
        }
        err = db.Ping()
        if err != nil {
                fmt.Println("Connected failed.")
                return err
        }
        db.SetMaxOpenConns(20) //设置数据库连接池最大连接数
        db.SetMaxIdleConns(10) //设置最大空闲连接数
        fmt.Println("Successfully connected!")
        return nil
}


func (pd *productDao) closeDB() (err error) {
        return db.Close()
}

func (pd *productDao) doQueryAll() (error, []product) {
        rows, err := db.Query(`Select * from product`)
        if err != nil {
                fmt.Println("Some amazing wrong happens in the process of Query.", err)
                return err, []product{}
        }
        products := make([]product, 0)
        defer rows.Close() //关闭连接
        index := 0
        var p product
        for rows.Next() {
                err := rows.Scan(&p.ProductNo, &p.Name, &p.Price)
                products = append(products, p)
                if err != nil { // 获得的都是字符串
                        fmt.Println("Some amazing wrong happens in the process of queryAll.", err)
                        return err, products
                }
                index++
        }
        if index > 0 {
                fmt.Println("The data of table is as follow.")
                for _, p := range products {
                        fmt.Printf("%v %s %v\n", p.ProductNo, p.Name, p.Price)
                }
                fmt.Println("Successfully query ", len(products))
                return nil, products
        } else {
                fmt.Println("No such data exists in database. ")
                return fmt.Errorf("No such data exists in database. "), products
        }
}




```

### 2.1.3 Sqlite

```go
//sqlite_manger.go
package main

import (
    "database/sql"
        "fmt"
        "time"
    _ "github.com/mattn/go-sqlite3"
)

func main() {
        // 打开/创建
        db, err := sql.Open("sqlite3", "./my.db")

        // 关闭
        defer db.Close()


        table := `
            CREATE TABLE IF NOT EXISTS user (
                    uid INTEGER PRIMARY KEY AUTOINCREMENT,
                        name VARCHAR(128) NULL,
                created DATE NULL
                );
                `
        _, err = db.Exec(table)
        if err != nil {
                panic(err)
                return ;
        }

        fmt.Println("CREATE TABLE Success!");


    stmt, err := db.Prepare("INSERT INTO user(name,  created) values(?,?)")
    if err != nil {
        panic(err)
    }
    // res 为返回结果//
    res, err := stmt.Exec("guoke", "2012-12-09")
    if err != nil {
        panic(err)
    }

    // 可以通过res取自动生成的id
    id, err := res.LastInsertId()
    if err != nil {
        panic(err)
    }
        fmt.Println(id);

        rows, err := db.Query("SELECT * FROM user")
    if err != nil {
          panic(err)
     }
    defer rows.Close()

    for rows.Next() {
        var uid int
        var name string
        var created time.Time
        err = rows.Scan(&uid, &name,  &created)
        if err != nil {
          panic(err)
        }

        fmt.Println(uid)
        fmt.Println(name)
        fmt.Println(created)
    }
}
```



## 2.2 net/http

### 2.2.1 简单的http响应

```go
//simpleHttpServer.go
package main

import (
        "os"
        "fmt"
        "log"
        "net/http"
        "reflect"
)

/* 打印类的所有属性 */
func GetClassAttribute(body interface{}) {
                var prop []string
                refType := reflect.TypeOf(body)
        if refType.Kind() != reflect.Struct {
                fmt.Println("Not a structure type.")
        }
        for i:=0;i<refType.NumField();i++ {
                field := refType.Field(i)
                if field.Anonymous {
                        prop = append(prop, field.Name)
                        for j := 0;j<field.Type.NumField();j++ {
                                prop = append(prop, field.Type.Field(j).Name)
                        }
                        continue
                }
                prop = append(prop, field.Name)
        }
        fmt.Printf("%v\n", prop)
}


func sayhelloName(w http.ResponseWriter, r *http.Request) {
   fmt.Println("打印Header参数列表：")
   if len(r.Header) > 0 {
      for k,v := range r.Header {
         fmt.Printf("%s = %s\n", k, v[0])
      }
   }
   fmt.Println("打印Form参数列表：")
   r.ParseForm()
   if len(r.Form) > 0 {
      for k,v := range r.Form {
         fmt.Printf("%s = %s\n", k, v[0])
      }
   }

   fmt.Println("打印r.URL信息：")
   fmt.Println("r.URL.Path: ",r.URL.Path)
   fmt.Println("r.URL.RawPath: ",r.URL.RawPath)
        GetClassAttribute(*r.URL)
   fmt.Println("===================================================\n\n")

        fmt.Fprintln(w, "hello world!")
}

func main() {
                pwd, _ := os.Getwd()
                fmt.Println("pwd: ",pwd)
        http.HandleFunc("/", sayhelloName)

        err := http.ListenAndServe(":12802", nil)

        if err != nil {
                log.Fatal("ListenAndServe:", err)

        }
}

```

### 2.2.1 解析json的post请求体参数

```go
//simpleHttpServer2.go
/*
解析Content-Type为 application/json的post请求体参数
*/
package main

import (
        "encoding/json"
        "fmt"
        "log"
        "net/http"
//      "io/ioutil"
)

type person_struct struct {
                FirstName string `json:"FirstName"`
                LastName  string `json:"LastName"`
}

/*
//读取json传递数据  data:'{"FirstName":"yd","LastName":"123456"}',
func test(rw http.ResponseWriter, req *http.Request) {
        fmt.Println("method:", req.Method)

        if req.Method != "POST" && req.Method != "post" {
                fmt.Fprintln(rw, "aaaa")
                return
        }

    var t person_struct

    body, err := ioutil.ReadAll(req.Body)
    if err != nil {
        fmt.Printf("read body err, %v\n", err)
        return
    }
    fmt.Println("req.Body: ", string(body) )

    //decoder := json.NewDecoder(req.Body)
    //err = decoder.Decode(&t)
    err = json.Unmarshal(body,&t)
    if err != nil {
        panic(err)
    }

    jsonBytes, err := json.Marshal(t)
    if err != nil {
       fmt.Println(err)
    }
    fmt.Println(string(jsonBytes))

    rw.Header().Set("Access-Control-Allow-Origin", "*")
    rw.Header().Set("Content-Type", "application/json;charset=utf-8")
    fmt.Fprintln(rw, string(jsonBytes) )



*/


func test(rw http.ResponseWriter, req *http.Request) {
                fmt.Println("method:", req.Method)

                if req.Method != "POST" && req.Method != "post" {
                                fmt.Fprintln(rw, "aaaa")
                                return
                }

        var t person_struct

        t.FirstName = req.FormValue("FirstName")
        t.LastName = req.FormValue("LastName")
        //转成json
        jsonBytes, err := json.Marshal(t)
    if err != nil {
           fmt.Println(err)
    }
    fmt.Println(string(jsonBytes))

        rw.Header().Set("Access-Control-Allow-Origin", "*")
    rw.Header().Set("Content-Type", "application/json;charset=utf-8")
        fmt.Fprintln(rw, string(jsonBytes) )
}

func indexpage(rw http.ResponseWriter, req *http.Request) {
        fmt.Fprintln(rw,"This is Index Page")
}

func main() {
        http.HandleFunc("/test", test)
        http.HandleFunc("/",indexpage)
        log.Fatal(http.ListenAndServe(":12802", nil))
}

```

### 2.2.3 静态资源与参数读取

```go
//
/*
参数读取
*/
package main

import (
    "fmt"
    "net/http"
)

func main() {
        // http://x.x.x.x:12800/?name=234&email=123@123.com
    http.HandleFunc("/", func (w http.ResponseWriter, r *http.Request) {
        fmt.Fprintln(w, "Welcome to my website!")
                fmt.Fprintln(w, r.URL.Query().Get("name"))
                fmt.Fprintln(w, r.FormValue("email"))
    })

        // http://x.x.x.x:12800/static/
    fs := http.FileServer(http.Dir("."))
    http.Handle("/static/", http.StripPrefix("/static/", fs))

    http.ListenAndServe(":12800", nil)
}

```
### 2.2.4 多端口侦听
```go
//simpleHttpServer4.go
/*
多端口侦听
*/
package main

import (
        "fmt"
        "net/http"
)

func index1(w http.ResponseWriter, r *http.Request) {
        _, _ = w.Write([]byte("This is server 1 ."))
}

func index2(w http.ResponseWriter, r *http.Request) {
    _, _ = w.Write([]byte("This is server 2 ."))
}


func http_server1() {
                server1 := http.NewServeMux()
                server1.HandleFunc("/", index1)
                err := http.ListenAndServe(":12801", server1)
                if err != nil {
                        fmt.Printf("http.ListenAndServe()函数执行错误,错误为:%v\n", err)
                        return
                }
                fmt.Println("Listen 12801 ...")
}

func http_server2() {
        server2 := http.NewServeMux()
        server2.HandleFunc("/", index2)
                err := http.ListenAndServe(":12802", server2)
        if err != nil {
            fmt.Printf("http.ListenAndServe()函数执行错误,错误为:%v\n", err)
            return
        }
                fmt.Println("Listen 12802 ...")

}

func main() {
                //以多线程方式开启server1
                go http_server1()
                //主进程开户server2, 主进程必须常驻运行，否则程序退出
                http_server2()
}

```

### 2.2.5 CGI包使用

```go
//simpleHttpServer5.go
/*
CGI包使用
*/
package main

import (
        "fmt"
        "log"
        "net/http"
        "net/http/cgi"
)


func test(rw http.ResponseWriter, req *http.Request) {
                fmt.Println(req.URL.Path)
                handler := new(cgi.Handler)
                /*
                /var/www/cgi-bin/test.sh :

                #!/bin/sh
                printf "Content-Type: text/plain;charset=utf-8\n\n"
                env

                */
                handler.Path = "/var/www/cgi-bin/test.sh"
                fmt.Println("handler.Path: ",handler.Path)
                handler.Dir = "/var/www/cgi-bin/"
                handler.ServeHTTP(rw, req)
}

func indexpage(rw http.ResponseWriter, req *http.Request) {
                fmt.Println(req.URL.Path)
        fmt.Fprintln(rw,"This is Index Page")
}

func main() {
        http.HandleFunc("/cgi-bin/test", test)
        http.HandleFunc("/",indexpage)
        log.Fatal(http.ListenAndServe(":12802", nil))
}

```

### 2.2.6 http.Request成员参数

```go
// sayhello project sayhello.go
package main

import (
        "fmt"
        "log"
        "net/http"
        "strings"
)

func sayhello(w http.ResponseWriter, r *http.Request) {
        // r.ParseForm()       //解析参数，默认是不会解析的
        //这些信息是输出到服务器端的打印信息
        fmt.Println("Request解析")
        //HTTP方法
        fmt.Println("method", r.Method)
        // RequestURI是被客户端发送到服务端的请求的请求行中未修改的请求URI
        fmt.Println("RequestURI", r.RequestURI)
        //URL类型,下方分别列出URL的各成员
        fmt.Println("URL_scheme", r.URL.Scheme)
        fmt.Println("URL_opaque", r.URL.Opaque)
        fmt.Println("URL_user", r.URL.User.String())
        fmt.Println("URL_host", r.URL.Host)
        fmt.Println("URL_path", r.URL.Path)
        fmt.Println("URL_RawQuery", r.URL.RawQuery)
        fmt.Println("URL_Fragment", r.URL.Fragment)
        //协议版本
        fmt.Println("proto", r.Proto)
        fmt.Println("protomajor", r.ProtoMajor)
        fmt.Println("protominor", r.ProtoMinor)
        //HTTP请求的头域
        for k, v := range r.Header {
                // fmt.Println("Header key:" + k)
                for _, vv := range v {
                        fmt.Println("header key:" + k + "  value:" + vv)
                }
        }
        //判断是否multipart方式
        is_multipart := false
        for _, v := range r.Header["Content-Type"] {
                if strings.Index(v, "multipart/form-data") != -1 {
                        is_multipart = true
                }
        }
        //解析body
        if is_multipart == true {
                r.ParseMultipartForm(128)
                fmt.Println("解析方式:ParseMultipartForm")
        } else {
                r.ParseForm()
                fmt.Println("解析方式:ParseForm")
        }
        //body内容长度
        fmt.Println("ContentLength", r.ContentLength)
        //是否在回复请求后关闭连接
        fmt.Println("Close", r.Close)
        //HOSt
        fmt.Println("host", r.Host)
        //form
        fmt.Println("Form", r.Form)
        //postform
        fmt.Println("PostForm", r.PostForm)
        /*
        //MultipartForm
        fmt.Println("MultipartForm", r.MultipartForm)
        //解析MultipartForm内的文件
        files := r.MultipartForm.File
        for k, v := range files {
                fmt.Println(k)
                for _, vv := range v {
                        fmt.Println(k + ":" + vv.Filename)
                }
        }
        */
        //该请求的来源地址
        fmt.Println("RemoteAddr", r.RemoteAddr)

        fmt.Println("\n=====================================================\n")
        fmt.Fprintf(w, "hello astaxie!") //这个写入到w的是输出到客户端的
}

func main() {
        http.HandleFunc("/", sayhello)
        err := http.ListenAndServe(":12800", nil)
        if err != nil {
                log.Fatal("ListenAndServe:", err)
        }
}

```

### 2.2.7 类httpd服务功能

> 以demo方式运行，支持静态html、cgi-bin。

```go
//goHttpd.go
package main

import (
        "flag"  //命令行参数
        "fmt"
        "log"
        "strings"
        "os"
        "strconv"
        "path"
        "net/http"
        "net/http/cgi"
)

var CigBinPrefix string
var CigBinPath   string
var FileStaticPath string
var ServerPort          string
var FileStaticPrefix string

//默认页面处理，用于支持cgi
func DealWithRequestForCgi(rw http.ResponseWriter, req *http.Request) {
                url_path := req.URL.Path
                log.Println("Receive "+req.Method+" from "+req.RemoteAddr+" ,URL.Path: "+url_path)
                //判断是否是cgi-bin请求
                if strings.Index(url_path, CigBinPrefix) == 0 {
                                handler := new(cgi.Handler)
                                handler.Path = CigBinPath+"/"+url_path[len(CigBinPrefix)-1:]
                                log.Println("handler.Path: "+handler.Path)
                                handler.Dir = CigBinPath
                                handler.ServeHTTP(rw, req)
                } else {
                                http.NotFound(rw, req)
                }
}

//开启Http服务
func StartHttpService() {
                http.HandleFunc("/", DealWithRequestForCgi)

                //设置静态文件路径
                fs := http.FileServer(http.Dir(FileStaticPath))
                http.Handle(FileStaticPrefix, http.StripPrefix(FileStaticPrefix, fs))

                log.Println("Server Listen on "+ServerPort+" ...")
                //开启http侦听
                err := http.ListenAndServe(":"+ServerPort, nil)
                if err != nil {
                                log.Println("ListenAndServe Error:", err)
                }
}

func init() {
                pid := os.Getpid()
            log.SetPrefix("[PID:"+strconv.Itoa(pid)+"] ")
                log.SetFlags(log.Ldate | log.Ltime | log.Lshortfile)
                //log.SetFlags(log.Ldate | log.Ltime )
}


func main() {
                flag.StringVar(&ServerPort, "p", "18000", "The Port for Httpd .")
                flag.StringVar(&FileStaticPath, "w", "/var/www/html/" , "The Path of WebSit .")
                flag.StringVar(&FileStaticPrefix, "s", "/myweb/", "The URL of WebSit .")
                flag.StringVar(&CigBinPath, "c", "/var/www/cgi-bin/", "The Path of Cgi-bin ." )
                flag.StringVar(&CigBinPrefix, "b", "/myweb/cgi-bin/", "The URL Path for Cgi-bin .")
                flag.Parse()

                if os.Getppid() != 1 {

                                createLogFile := func (fileName string) (fd *os.File, err error) {
                                                dir := path.Dir(fileName)
                                                if _, err = os.Stat(dir); err != nil && os.IsNotExist(err) {
                                                                if err = os.MkdirAll(dir, 0755); err != nil {
                                                                                fmt.Printf("Start-Daemon: create dir: %s failed, err is: %v\n", dir, err)
                                                                                return
                                                                }
                                                }
                                                if fd, err = os.OpenFile(fileName,os.O_RDWR|os.O_CREATE|os.O_APPEND,os.ModeAppend) ; err != nil {
                                                //if fd, err = os.Create(fileName); err != nil {
                                                                fmt.Printf("Start-Daemon: open log file: %s failed, err is: %v\n", fileName, err)
                                                                return
                                                }
                                                return
                                }

                                logFd, err := createLogFile("/var/log/goHttpd.log")
                                if err != nil {
                                                return
                                }
                                defer logFd.Close()

                                cmdName := os.Args[0]
                                newProc, err := os.StartProcess(cmdName, os.Args, &os.ProcAttr{Files: []*os.File{logFd, logFd, logFd}})
                                if err != nil {
                                                log.Println("Start-Deamon: start process: ", cmdName," failed, err is: ", err)
                                                return
                                }
                                log.Println("Start-Deamon: run in daemon success, pid:", newProc.Pid)
                                return
                }

                log.Println("Config ServerPort:", ServerPort)
                log.Println("Config FileStaticPrefix:", FileStaticPrefix)
                log.Println("Config FileStaticPath:", FileStaticPath )
                log.Println("Config CigBinPrefix:", CigBinPrefix )
                log.Println("Config CigBinPath:", CigBinPath)
                //log.Println("Config :",)

                StartHttpService()
}

```
+ goHttpd.service 服务配置文件
```shell
#把hxcl.service放到 /usr/lib/systemd/system 目录下 /usr/lib/systemd/system
#命令
#systemctl start hxcl.service 启动
#systemctl stop hxcl.service 停止
#systemctl restart hxcl.service 重启
#systemctl enable hxcl.service 添加为系统自启动服务



#[Unit]部分主要是对这个服务的说明，内容包括Description和After，Description
#用于描述服务，After用于描述服务类别
[Unit]
Description=CBI Service
After=network.service

#[Service]部分是服务的关键，是服务的一些具体运行参数的设置，这里Type=forking
#是后台运行的形式，PIDFile为存放PID的文件路径，ExecStart为服务的具体运行命令，
#ExecReload为重启命令，ExecStop为停止命令，PrivateTmp=True表示给服务分配独
#立的临时空间，注意：[Service]部分的启动、重启、停止命令全部要求使用绝对路径，使
#用相对路径则会报错！
#StandardOutput=null 是将程序业务日志输出到空，也可以指定文件，或者交给journal处理

[Service]
#Type=forking
User=root
Group=root
WorkingDirectory=/home/golang/http/
ExecStart=/home/golang/http/goHttpd >/dev/null 2>&1 &
#SuccessExitStatus=143
ExecStop=/usr/bin/kill -9 $MAINPID
Environment=HOME=/home/golang/http/ PWD=/home/golang/http/
StandardOutput=null
#StandardOutput=/tmp/services/logs/iBot/iBot-run.log

#[Install]部分是服务安装的相关设置，可设置为多用户的
[Install]
WantedBy=multi-user.target


```

### 2.2.8 Http Client

```go
//http_client.go
package main

import (
    "bytes"
//    "encoding/json"
    "io"
    "io/ioutil"
    "net/http"
    "net/url"
    "time"
    "fmt"
    "strings"
)

// 发送GET请求
// url：         请求地址
// response：    请求返回的内容
func Get(url string) string {

    // 超时时间：5秒
    client := &http.Client{Timeout: 5 * time.Second}
    resp, err := client.Get(url)
    if err != nil {
        panic(err)
    }
    defer resp.Body.Close()
    var buffer [512]byte
    result := bytes.NewBuffer(nil)
    for {
        n, err := resp.Body.Read(buffer[0:])
        result.Write(buffer[0:n])
        if err != nil && err == io.EOF {
            break
        } else if err != nil {
            panic(err)
        }
    }

    return result.String()
}

// 发送POST请求
// url：         请求地址
// data：        POST请求提交的数据
// contentType： 请求体格式，如：application/json
// content：     请求放回的内容
func Post(url string, data url.Values, contentType string) string {

    // 超时时间：5秒
    client := &http.Client{Timeout: 5 * time.Second}
    //jsonStr, _ := json.Marshal(data)
    resp, err := client.Post(url, contentType, strings.NewReader(data.Encode()))
    if err != nil {
        panic(err)
    }
    defer resp.Body.Close()

    result, _ := ioutil.ReadAll(resp.Body)
    return string(result)
}

func main() {
        fmt.Println(Get("http://www.01happy.com/demo/accept.php?id=1"))
        fmt.Println("-----------------------------")
//      data := map[string]string{"France": "Paris", "Italy": "Rome", "Japan": "Tokyo", "India": "New delhi"}
        data := url.Values{"app_id":{"238b2213-a8ca-42d8-8eab-1f1db3c50ed6"}, "mobile_tel":{"13794227450"}}
        fmt.Println(Post("http://www.01happy.com/demo/accept.php",data,"application/x-www-form-urlencoded"))
}

```

## 2.3 Socket

### 2.3.1 Server

```go
//server.go
package main

import (
        "bufio"
        "fmt"
        "flag"
        "log"
        "net"
)

func handleConnection(conn net.Conn) {
        defer conn.Close()
        fmt.Fprintf(conn, "Input q to quit !\r\n")
        for {
                data, err := bufio.NewReader(conn).ReadString('\n')
                if err != nil {
                        log.Fatal(err)
                }
                fmt.Print(conn.RemoteAddr()," ---> ",string(data))
                if string(data)[0] == 'q' {
                                break
                }
                fmt.Fprintf(conn, string(data))
        }
}

func main() {
                var port string
                flag.StringVar(&port, "p", "3371",  "Port for Server !")
                flag.Parse()

                fmt.Println("Listen on "+port+" ... ")
        l, err := net.Listen("tcp", ":"+port)
        if err != nil {
                log.Fatal(err)
        }
        defer l.Close()
        for {
                conn, err := l.Accept()
                if err != nil {
                        log.Fatal(err)
                }
                go handleConnection(conn)
        }
}

```

### 2.3.2 Client

```go
//client.go
package main

import (
    "bufio"
    "fmt"
    "log"
    "net"
)

func main() {
    conn, err := net.Dial("tcp", ":3371")
    if err != nil {
        log.Fatal(err)
    }
    defer conn.Close()
    fmt.Fprintf(conn, "hello\n")
    res, err := bufio.NewReader(conn).ReadString('\n')
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println(string(res))
    fmt.Fprintf(conn, "Jimmy!\n")
}

```

## 2.4 日志模块

### 2.4.1 log模块

```go
//test2.go
package main

import (
    "log"
)

func init() {
    log.SetPrefix("TRACE: ")
    log.SetFlags(log.Ldate | log.Ltime | log.Lshortfile)
    log.SetFlags(log.Ldate | log.Ltime )
}

func main() {
    // Println writes to the standard logger.
    log.Println("message")

    // Fatalln is Println() followed by a call to os.Exit(1).
//    log.Fatalln("fatal message")

    // Panicln is Println() followed by a call to panic().
//    log.Panicln("panic message")
}

```

```shell
[root@5d09d6b9f9d5 seelog]# go run test2.go
TRACE: 2022/03/17 13:30:16 message
[root@5d09d6b9f9d5 seelog]#
```

### 2.4.2 seelog直接模式

```go
//test1.go
package main

import (
    seelog "github.com/cihub/seelog"
)

func main() {
                seelog.Error("seelog error")
                seelog.Info("seelog info")
                seelog.Debug("seelog debug")
                seelog.Flush() //输出
}
```

```shell
[root@5d09d6b9f9d5 seelog]# go run test1.go
1647524182010802520 [Error] seelog error
1647524182010826318 [Info] seelog info
1647524182010831618 [Debug] seelog debug
[root@5d09d6b9f9d5 seelog]#
```

### 2.4.3 seelog配置文件模式

```go
 TestSeelog.go
// TestSeelog.go
package main

import (
    seelog "github.com/cihub/seelog"
)

func main() {
    logger, err := seelog.LoggerFromConfigAsFile("./logconfig.xml")

    if err != nil {
        seelog.Critical("err parsing config log file", err)
        return
    }
    seelog.ReplaceLogger(logger)

    seelog.Error("seelog error")
    seelog.Info("seelog info")
    seelog.Debug("seelog debug")
        seelog.Flush()
}
```

+ logconfig.xml

```xml
<seelog levels="trace,debug,info,warn,error,critical">
    <outputs formatid="main">
        <!-- 对控制台输出的Log按级别分别用颜色显示。6种日志级别我仅分了三组颜色，如果想每个级别都用不同颜色则需要简单修改即可 -->
        <filter levels="trace,debug,info">
            <console formatid="colored-default"/>
        </filter>
        <filter levels="warn">
            <console formatid="colored-warn"/>
        </filter>
        <filter levels="error,critical">
            <console formatid="colored-error"/>
        </filter>

        <!-- 将日志输出到磁盘文件，按文件大小进行切割日志，单个文件最大100M，最多5个日志文件 -->
        <rollingfile formatid="main" type="size" filename="./log/default.log" maxsize="104857600" maxrolls="5" />
    </outputs>
    <formats>
        <format id="colored-default"  format="[%Date %Time] [%LEVEL] [%File:%Line] %Msg%n"/>
        <format id="colored-warn"  format="[%Date %Time] [%LEVEL] [%File:%Line] %Msg%n"/>
        <format id="colored-error"  format="[%Date %Time] [%LEVEL] [%File:%Line] %Msg%n"/>
        <format id="main" format="[%Date %Time] [%LEVEL] [%File:%Line] %Msg%n"/>
    </formats>
</seelog>
```

+ 输出结果

```bash
[root@5d09d6b9f9d5 seelog]# go run TestSeelog.go
[2022-03-17 13:39:33] [ERROR] [TestSeelog.go:18] seelog error
[2022-03-17 13:39:33] [INFO] [TestSeelog.go:19] seelog info
[2022-03-17 13:39:33] [DEBUG] [TestSeelog.go:20] seelog debug
[root@5d09d6b9f9d5 seelog]#
```

# 3 Gin框架

> - Gin是一个golang的微框架，封装比较优雅，API友好，源码注释比较明确，具有快速灵活，容错方便等特点
> - 对于golang而言，web框架的依赖要远比Python，Java之类的要小。自身的`net/http`足够简单，性能也非常不错
> - 借助框架开发，不仅可以省去很多常用的封装带来的时间，也有助于团队的编码风格和形成规范

## 3.1 安装Gin

1. 首先需要安装Go（需要1.10+版本），然后可以使用下面的Go命令安装Gin。

> go get -u github.com/gin-gonic/gin

2. 将其导入您的代码中：

> import "github.com/gin-gonic/gin"

3. （可选）导入net/http。例如，如果使用常量，则需要这样做http.StatusOK。

> import "net/http"

4. helloword

```go
//01.go
package main

import (
    "net/http"
    "github.com/gin-gonic/gin"
)

func main() {
    // 1.创建路由
   r := gin.Default()
   // 2.绑定路由规则，执行的函数
   // gin.Context，封装了request和response
   r.GET("/", func(c *gin.Context) {
      c.String(http.StatusOK, "hello World!")
   })
   // 3.监听端口，默认在8080
   // Run("里面不指定端口号默认为8080")
   r.Run(":8000")
}

```

## 3.2 Gin路由

1. 基本路由

> - gin 框架中采用的路由库是基于httprouter做的
> - 地址为：https://github.com/julienschmidt/httprouter
>

```go
//02.go
package main

import (
    "net/http"

    "github.com/gin-gonic/gin"
)

func main() {
    r := gin.Default()
    r.GET("/", func(c *gin.Context) {
        c.String(http.StatusOK, "hello word")
    })
    r.POST("/xxxpost",func(c *gin.Context) {
        c.String(http.StatusOK, "hello word, post")
    })
    r.PUT("/xxxput")
    //监听端口默认为8080
    r.Run(":8000")
}
```

2. Restful风格的API
> gin支持Restful风格的API: 即Representational State Transfer的缩写。直接翻译的意思是"表现层状态转化"，是一种互联网应用程序的API设计理念：URL定位资源，用HTTP描述操作

3. API参数
> 可以通过Context的Param方法来获取API参数，例如：localhost:8000/user/worden/info
> 
```go
//03.go
package main

import (
    "net/http"
    "strings"

    "github.com/gin-gonic/gin"
)

func main() {
    r := gin.Default()
    r.GET("/user/:name/*action", func(c *gin.Context) {
        name := c.Param("name")
        action := c.Param("action")
        //截取/
        action = strings.Trim(action, "/")
        c.String(http.StatusOK, name+" is "+action)
    })
    //默认为监听8080端口
    r.Run(":8000")
}
```

```shell
[root@Docker1 golang]# curl http://192.168.1.241:8000/user/worden/info
worden is info
```

4. URL参数

> - API ? name=zs，URL参数可以通过DefaultQuery()或Query()方法获取
> - DefaultQuery()若参数不村则，返回默认值，Query()若不存在，返回空串

```go
//04.go 
package main

import (
    "fmt"
    "net/http"

    "github.com/gin-gonic/gin"
)

func main() {
    r := gin.Default()
    r.GET("/user", func(c *gin.Context) {
        //指定默认值
        //http://localhost:8080/user 才会打印出来默认的值
        name := c.DefaultQuery("name", "枯藤")
        c.String(http.StatusOK, fmt.Sprintf("hello %s", name))
    })
    r.Run(":8000")
}
```

```shell
[root@Docker1 golang]# curl http://192.168.1.241:8000/user
hello 枯藤
[root@Docker1 golang]# curl http://192.168.1.241:8000/user?name=worden
hello worden
```

5. 表单参数

> - 表单传输为post请求，http常见的传输格式为四种：
>   - application/json
>   - application/x-www-form-urlencoded
>   - application/xml
>   - multipart/form-data
> - 表单参数可以通过PostForm()方法获取，该方法默认解析的是x-www-form-urlencoded或from-data格式的参数

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <form action="http://192.168.1.241:8000/form" method="post" action="application/x-www-form-urlencoded">
        用户名：<input type="text" name="username" placeholder="请输入你的用户名">  <br>
        密&nbsp;&nbsp;&nbsp;码：<input type="password" name="userpassword" placeholder="请输入你的密码">  <br>
        <input type="submit" value="提交">
    </form>
</body>
</html>
```

```go
//05.go
package main

import (
    "fmt"
    "net/http"

    "github.com/gin-gonic/gin"
)

func main() {
    r := gin.Default()
    r.POST("/form", func(c *gin.Context) {
        types := c.DefaultPostForm("type", "post")
        username := c.PostForm("username")
        password := c.PostForm("userpassword")
        // c.String(http.StatusOK, fmt.Sprintf("username:%s,password:%s,type:%s", username, password, types))
        c.String(http.StatusOK, fmt.Sprintf("username:%s,password:%s,type:%s", username, password, types))
    })
    r.Run(":8000")
}
```

```shell
[root@Docker1 golang]# curl 'http://192.168.1.241:8000/form'   -H 'Content-Type: application/x-www-form-urlencoded'   -d 'username=user1&userpassword=admin1'
username:user1,password:admin1,type:post
```

6. 文件单个上传

> - multipart/form-data格式用于文件上传
> - gin文件上传与原生的net/http方法类似，不同在于gin把原生的request封装到c.Request中

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <form action="http://192.168.1.241:8080/upload" method="post" enctype="multipart/form-data">
          上传文件:<input type="file" name="file" >
          <input type="submit" value="提交">
    </form>
</body>
</html>
```

```go
//06.go
package main

import (
    "net/http"
    "github.com/gin-gonic/gin"
)

func main() {
    r := gin.Default()
    //限制上传最大尺寸
    r.MaxMultipartMemory = 8 << 20
    r.POST("/upload", func(c *gin.Context) {
        file, err := c.FormFile("file")
        if err != nil {
            c.String(500, "上传文件出错")
        }
        // c.JSON(200, gin.H{"message": file.Header.Context})
        c.SaveUploadedFile(file, file.Filename)
        c.String(http.StatusOK, file.Filename)
    })
    r.Run()
}
```

7. 上传特定文件

>有的上传文件需要限制上传文件的类型以及上传文件的大小，但是gin框架暂时没有这些函数(也有可能是我没找到)，因此基于原生的函数写了一个可以限制大小以及文件类型的上传函数

```go
//07.go
package main

import (
    "log"
    "net/http"
    "github.com/gin-gonic/gin"
)

func main() {
    r := gin.Default()
    r.POST("/upload", func(c *gin.Context) {
        _, headers, err := c.Request.FormFile("file")
        if err != nil {
            log.Printf("Error when try to get file: %v", err)
        }
        //headers.Size 获取文件大小
        if headers.Size > 1024*1024*2 {
            c.String(http.StatusOK,"文件太大了")
            return
        }
        //headers.Header.Get("Content-Type")获取上传文件的类型
        if headers.Header.Get("Content-Type") != "image/png" {
            c.String(http.StatusOK,"只允许上传png图片")
            return
        }
        c.SaveUploadedFile(headers, "./video/"+headers.Filename)
        c.String(http.StatusOK, headers.Filename)
    })
    r.Run()
}
```

8. 上传多个文件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <form action="http://192.168.1.241:8080/upload" method="post" enctype="multipart/form-data">
          上传文件:<input type="file" name="files" multiple>
          <input type="submit" value="提交">
    </form>
</body>
</html>
```

```go
//08.go
package main

import (
   "github.com/gin-gonic/gin"
   "net/http"
   "fmt"
)

// gin的helloWorld

func main() {
   // 1.创建路由
   // 默认使用了2个中间件Logger(), Recovery()
   r := gin.Default()
   // 限制表单上传大小 8MB，默认为32MB
   r.MaxMultipartMemory = 8 << 20
   r.POST("/upload", func(c *gin.Context) {
      form, err := c.MultipartForm()
      if err != nil {
         c.String(http.StatusBadRequest, fmt.Sprintf("get err %s", err.Error()))
      }
      // 获取所有文件
      files := form.File["files"]
      // 遍历所有文件
      for _, file := range files {
         // 逐个存
         if err := c.SaveUploadedFile(file, file.Filename); err != nil {
            c.String(http.StatusBadRequest, fmt.Sprintf("upload err %s", err.Error()))
            return
         }
      }
      c.String(http.StatusOK, fmt.Sprintf("upload ok %d files", len(files)))
   })
   //默认端口号是8080
   r.Run()
}
```

9. routes group是为了管理一些相同的URL

```go
//09.go 
package main

import (
   "github.com/gin-gonic/gin"
   "fmt"
)

// gin的helloWorld

func main() {
   // 1.创建路由
   // 默认使用了2个中间件Logger(), Recovery()
   r := gin.Default()
   // 路由组1 ，处理GET请求
   v1 := r.Group("/v1")
   // {} 是书写规范
   {
      v1.GET("/login", login)
      v1.GET("submit", submit)
   }
   v2 := r.Group("/v2")
   {
      v2.POST("/login", login)
      v2.POST("/submit", submit)
   }
   r.Run(":8000")
}

func login(c *gin.Context) {
   name := c.DefaultQuery("name", "jack")
   c.String(200, fmt.Sprintf("hello %s\n", name))
}

func submit(c *gin.Context) {
   name := c.DefaultQuery("name", "lily")
   c.String(200, fmt.Sprintf("hello %s\n", name))
}
```

```shell
[root@Docker1 gin]# curl http://192.168.1.241:8000/v1/login
hello jack
[root@Docker1 gin]# curl http://192.168.1.241:8000/v2/login
404 page not found
[root@Docker1 gin]# curl http://192.168.1.241:8000/v2/login -X POST
hello jack
```

10. 路由拆分

```go
//10.go
package main

import (
    "net/http"
    "fmt"
    "github.com/gin-gonic/gin"
)

func helloHandler(c *gin.Context) {
    c.JSON(http.StatusOK, gin.H{
        "message": "Hello www.topgoer.com!",
    })
}

func setupRouter() *gin.Engine {
    r := gin.Default()
    r.GET("/topgoer", helloHandler)
    return r
}

func main() {
    r := setupRouter()
    if err := r.Run(); err != nil {
        fmt.Println("startup service failed, err:%v\n", err)
    }
}
```

## 3.3 Gin 数据解析和绑定

1. Json 数据解析和绑定

```go
//11.go
package main

import (
   "github.com/gin-gonic/gin"
   "net/http"
)

// 定义接收数据的结构体
type Login struct {
   // binding:"required"修饰的字段，若接收为空值，则报错，是必须字段
   User    string `form:"username" json:"user" uri:"user" xml:"user" binding:"required"`
   Pssword string `form:"password" json:"password" uri:"password" xml:"password" binding:"required"`
}

func main() {
   // 1.创建路由
   r := gin.Default()
   // JSON绑定
   r.POST("loginJSON", func(c *gin.Context) {
      // 声明接收的变量
      var json Login
      // 将request的body中的数据，自动按照json格式解析到结构体
      if err := c.ShouldBindJSON(&json); err != nil {
         // 返回错误信息
         // gin.H封装了生成json数据的工具
         c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
         return
      }
      // 判断用户名密码是否正确
      if json.User != "root" || json.Pssword != "admin" {
         c.JSON(http.StatusBadRequest, gin.H{"status": "304"})
         return
      }
      c.JSON(http.StatusOK, gin.H{"status": "200"})
   })
   r.Run(":8000")
}
```

```shell
[root@Docker1 gin]# curl http://192.168.1.241:8000/loginJSON -H "Content-Type: application/json"  -d '{"user":"root", "password":"admin"}'  -X POST
{"status":"200"}
[root@Docker1 gin]# curl http://192.168.1.241:8000/loginJSON -H "Content-Type: application/json"  -d '{"user":"root", "password":"admin1"}'  -X POST
{"status":"304"}
```

2. 表单数据解析和绑定

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <form action="http://192.168.1.241:8000/loginForm" method="post" enctype="application/x-www-form-urlencoded">
        用户名<input type="text" name="username"><br>
        密码<input type="password" name="password">
        <input type="submit" value="提交">
    </form>
</body>
</html>
```

```go
//12.go
package main

import (
    "net/http"

    "github.com/gin-gonic/gin"
)

// 定义接收数据的结构体
type Login struct {
    // binding:"required"修饰的字段，若接收为空值，则报错，是必须字段
    User    string `form:"username" json:"user" uri:"user" xml:"user" binding:"required"`
    Pssword string `form:"password" json:"password" uri:"password" xml:"password" binding:"required"`
}

func main() {
    // 1.创建路由
    r := gin.Default()
    // JSON绑定
    r.POST("/loginForm", func(c *gin.Context) {
        // 声明接收的变量
        var form Login
        // Bind()默认解析并绑定form格式
        // 根据请求头中content-type自动推断
        if err := c.Bind(&form); err != nil {
            c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
            return
        }
        // 判断用户名密码是否正确
        if form.User != "root" || form.Pssword != "admin" {
            c.JSON(http.StatusBadRequest, gin.H{"status": "304"})
            return
        }
        c.JSON(http.StatusOK, gin.H{"status": "200"})
    })
    r.Run(":8000")
}
```

3. URI数据解析和绑定

```go
//13.go
package main

import (
    "net/http"

    "github.com/gin-gonic/gin"
)

// 定义接收数据的结构体
type Login struct {
    // binding:"required"修饰的字段，若接收为空值，则报错，是必须字段
    User    string `form:"username" json:"user" uri:"user" xml:"user" binding:"required"`
    Pssword string `form:"password" json:"password" uri:"password" xml:"password" binding:"required"`
}

func main() {
    // 1.创建路由
    r := gin.Default()
    // JSON绑定
    r.GET("/:user/:password", func(c *gin.Context) {
        // 声明接收的变量
        var login Login
        // 将uri的数据解析到结构体
        if err := c.ShouldBindUri(&login); err != nil {
            c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
            return
        }
        // 判断用户名密码是否正确
        if login.User != "root" || login.Pssword != "admin" {
            c.JSON(http.StatusBadRequest, gin.H{"status": "304"})
            return
        }
        c.JSON(http.StatusOK, gin.H{"status": "200"})
    })
    r.Run(":8000")
}
```

```shell
[root@Docker1 bin]# curl  http://192.168.1.241:8000/admin/test
{"status":"304"}
[root@Docker1 bin]# curl  http://192.168.1.241:8000/root/admin
{"status":"200"}
```

##  3.4 gin 渲染

1. 各种数据格式的响应

> json、结构体、XML、YAML类似于java的properties、ProtoBuf

```go
//14.go
package main

import (
    "github.com/gin-gonic/gin"
    "github.com/gin-gonic/gin/testdata/protoexample"
)

// 多种响应方式
func main() {
    // 1.创建路由
    // 默认使用了2个中间件Logger(), Recovery()
    r := gin.Default()
    // 1.json
    r.GET("/someJSON", func(c *gin.Context) {
        c.JSON(200, gin.H{"message": "someJSON", "status": 200})
    })
    // 2. 结构体响应
    r.GET("/someStruct", func(c *gin.Context) {
        var msg struct {
            Name    string
            Message string
            Number  int
        }
        msg.Name = "root"
        msg.Message = "message"
        msg.Number = 123
        c.JSON(200, msg)
    })
    // 3.XML
    r.GET("/someXML", func(c *gin.Context) {
        c.XML(200, gin.H{"message": "abc"})
    })
    // 4.YAML响应
    r.GET("/someYAML", func(c *gin.Context) {
        c.YAML(200, gin.H{"name": "zhangsan"})
    })
    // 5.protobuf格式,谷歌开发的高效存储读取的工具
    // 数组？切片？如果自己构建一个传输格式，应该是什么格式？
    r.GET("/someProtoBuf", func(c *gin.Context) {
        reps := []int64{int64(1), int64(2)}
        // 定义数据
        label := "label"
        // 传protobuf格式数据
        data := &protoexample.Test{
            Label: &label,
            Reps:  reps,
        }
        c.ProtoBuf(200, data)
    })

    r.Run(":8000")
}
```

```shell
[root@Docker1 bin]# curl  http://192.168.1.241:8000/someJSON
{"message":"someJSON","status":200}
[root@Docker1 bin]# curl  http://192.168.1.241:8000/someStruct
{"Name":"root","Message":"message","Number":123}
[root@Docker1 bin]# curl  http://192.168.1.241:8000/someXML
<map><message>abc</message></map>
[root@Docker1 bin]# curl  http://192.168.1.241:8000/someYAML
name: zhangsan
[root@Docker1 bin]# curl  http://192.168.1.241:8000/someProtoBuf

label[root@Docker1 bin]# xterm-256color

```

2. HTML模板渲染

> - gin支持加载HTML模板, 然后根据模板参数进行配置并返回相应的数据，本质上就是字符串替换
> - LoadHTMLGlob()方法可以加载模板文件

+ tem/index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>{{.title}}</title>
</head>
    <body>
        fgkjdskjdsh{{.ce}}
    </body>
</html>
```

```go
//15.go
package main

import (
    "net/http"

    "github.com/gin-gonic/gin"
)

func main() {
    r := gin.Default()
    r.LoadHTMLGlob("tem/*")
    r.GET("/index", func(c *gin.Context) {
        c.HTML(http.StatusOK, "index.html", gin.H{"title": "我是测试", "ce": "123456"})
    })
    r.Run()
}
```

```shell
[root@Docker1 gin]# curl http://192.168.1.241:8080/index
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>我是测试</title>
</head>
    <body>
        fgkjdskjdsh123456
    </body>
</html>
[root@Docker1 gin]#
```

-----

+ 头尾分离

  + tem/user/index.html

  ```html
  {{ define "user/index.html" }}
  {{template "public/header" .}}
          fgkjdskjdsh{{.address}}
  {{template "public/footer" .}}
  {{ end }}
  ```

  + tem/public/header.html

  ```html
  {{define "public/header"}}
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <title>{{.title}}</title>
  </head>
      <body>
  
  {{end}}
  ```

  + tem/public/footer.html

  ```html
  {{define "public/footer"}}
  </body>
  </html>
  {{ end }}
  ```

  + go代码

  ```go
  //16.go
  package main
  
  import (
      "net/http"
      "github.com/gin-gonic/gin"
  )
  
  func main() {
      r := gin.Default()
      //静态文件目录
      //r.Static("/assets", "./assets")
      
      r.LoadHTMLGlob("tem/**/*")
      r.GET("/index", func(c *gin.Context) {
          c.HTML(http.StatusOK, "user/index.html", gin.H{"title": "我是测试", "address": "www.5lmh.com"})
      })
      r.Run()
  }
  ```

  + 结果

  ```shell
  [root@Docker1 gin]# curl http://192.168.1.241:8080/index
  
  
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <title>我是测试</title>
  </head>
      <body>
  
  
          fgkjdskjdshwww.5lmh.com
  
  </body>
  </html>
  
  [root@Docker1 gin]#
  ```

---

3. 重定向Redirect

```go
//17.go
package main

import (
    "net/http"
    "github.com/gin-gonic/gin"
)

func main() {
    r := gin.Default()
    r.GET("/index", func(c *gin.Context) {
        c.Redirect(http.StatusMovedPermanently, "http://www.5lmh.com")
    })
    r.Run()
}
```
```shell
[root@Docker1 gin]# curl -v  http://192.168.1.241:8080/index
* About to connect() to 192.168.1.241 port 8080 (#0)
*   Trying 192.168.1.241...
* Connected to 192.168.1.241 (192.168.1.241) port 8080 (#0)
> GET /index HTTP/1.1
> User-Agent: curl/7.29.0
> Host: 192.168.1.241:8080
> Accept: */*
>
< HTTP/1.1 301 Moved Permanently
< Content-Type: text/html; charset=utf-8
< Location: http://www.5lmh.com
< Date: Sun, 20 Mar 2022 15:49:24 GMT
< Content-Length: 54
<
<a href="http://www.5lmh.com">Moved Permanently</a>.

* Connection #0 to host 192.168.1.241 left intact
[root@Docker1 gin]#
```


4. 同步异步

```go
//18.go
package main

import (
    "log"
    "time"

    "github.com/gin-gonic/gin"
)

func main() {
    // 1.创建路由
    // 默认使用了2个中间件Logger(), Recovery()
    r := gin.Default()
    // 1.异步
    r.GET("/long_async", func(c *gin.Context) {
        // 需要搞一个副本
        copyContext := c.Copy()
        // 异步处理
        go func() {
            time.Sleep(3 * time.Second)
            log.Println("异步执行：" + copyContext.Request.URL.Path)
        }()
    })
    // 2.同步
    r.GET("/long_sync", func(c *gin.Context) {
        time.Sleep(3 * time.Second)
        log.Println("同步执行：" + c.Request.URL.Path)
    })

    r.Run(":8000")
}

```

```shell
[root@Docker1 gin]# export GIN_MODE=release
[root@Docker1 gin]# go run 18.go
[GIN] 2022/03/20 - 23:54:07 | 200 |         5.4µs |   192.168.1.241 | GET      "/long_async"
2022/03/20 23:54:10 异步执行：/long_async
2022/03/20 23:54:22 同步执行：/long_sync
[GIN] 2022/03/20 - 23:54:22 | 200 |  3.000348935s |   192.168.1.241 | GET      "/long_sync"

```

## 3.5 gin 中间件

1. 全局中间件
> 所有请求都经过此中间件，通用Use()函数注册

```go
//19.go
package main

import (
    "fmt"
    "time"

    "github.com/gin-gonic/gin"
)

// 定义中间
func MiddleWare() gin.HandlerFunc {
    return func(c *gin.Context) {
        t := time.Now()
        fmt.Println("中间件开始执行了")
        // 设置变量到Context的key中，可以通过Get()取
        c.Set("request", "中间件")
        status := c.Writer.Status()
        fmt.Println("中间件执行完毕", status)
        t2 := time.Since(t)
        fmt.Println("time:", t2)
    }
}

func main() {
    // 1.创建路由
    // 默认使用了2个中间件Logger(), Recovery()
    r := gin.Default()
    // 注册中间件
    r.Use(MiddleWare())
    // {}为了代码规范
    {
        r.GET("/ce", func(c *gin.Context) {
            // 取值
            req, _ := c.Get("request")
            fmt.Println("request:", req)
            // 页面接收
            c.JSON(200, gin.H{"request": req})
        })

    }
    r.Run()
}
```

```shell
[GIN-debug] Listening and serving HTTP on :8080
中间件开始执行了
中间件执行完毕 404
time: 12.999µs
[GIN] 2022/03/21 - 00:02:08 | 404 |      22.599µs |   192.168.1.241 | GET      "/long_sync"
中间件开始执行了
中间件执行完毕 200
time: 12.599µs
request: 中间件
[GIN] 2022/03/21 - 00:02:58 | 200 |      72.095µs |   192.168.1.241 | GET      "/ce"
```

2. Next()方法

> Next() 返回函数先执行处理代码，再回到中间件执行后续代码

```go
//20.go
package main

import (
    "fmt"
    "time"

    "github.com/gin-gonic/gin"
)

// 定义中间
func MiddleWare() gin.HandlerFunc {
    return func(c *gin.Context) {
        t := time.Now()
        fmt.Println("中间件开始执行了")
        // 设置变量到Context的key中，可以通过Get()取
        c.Set("request", "中间件")
        // 执行函数
        c.Next()
        // 中间件执行完后续的一些事情
        status := c.Writer.Status()
        fmt.Println("中间件执行完毕", status)
        t2 := time.Since(t)
        fmt.Println("time:", t2)
    }
}

func main() {
    // 1.创建路由
    // 默认使用了2个中间件Logger(), Recovery()
    r := gin.Default()
    // 注册中间件
    r.Use(MiddleWare())
    // {}为了代码规范
    {
        r.GET("/ce", func(c *gin.Context) {
            // 取值
            req, _ := c.Get("request")
            fmt.Println("request:", req)
            // 页面接收
            c.JSON(200, gin.H{"request": req})
        })

    }
    r.Run()
}
```

```shell
[GIN-debug] Listening and serving HTTP on :8080
中间件开始执行了
request: 中间件
中间件执行完毕 200
time: 44.897µs
[GIN] 2022/03/21 - 00:09:46 | 200 |      51.497µs |   192.168.1.241 | GET      "/ce"
```

3. 局部中间件

> 在路由函数中，中间件作参数使用

```go
package main

import (
    "fmt"
    "time"

    "github.com/gin-gonic/gin"
)

// 定义中间
func MiddleWare() gin.HandlerFunc {
    return func(c *gin.Context) {
        t := time.Now()
        fmt.Println("中间件开始执行了")
        // 设置变量到Context的key中，可以通过Get()取
        c.Set("request", "中间件")
        // 执行函数
        c.Next()
        // 中间件执行完后续的一些事情
        status := c.Writer.Status()
        fmt.Println("中间件执行完毕", status)
        t2 := time.Since(t)
        fmt.Println("time:", t2)
    }
}

func main() {
    // 1.创建路由
    // 默认使用了2个中间件Logger(), Recovery()
    r := gin.Default()
    //局部中间件使用
    r.GET("/ce", MiddleWare(), func(c *gin.Context) {
        // 取值
        req, _ := c.Get("request")
        fmt.Println("request:", req)
        // 页面接收
        c.JSON(200, gin.H{"request": req})
    })
    r.Run()
}
```

4. 推荐中间件

https://github.com/gin-gonic/contrib/blob/master/README.md

## 3.6 会话控制

1. SetCookie

```go
package main

import (
   "github.com/gin-gonic/gin"
   "fmt"
)

func main() {
   // 1.创建路由
   // 默认使用了2个中间件Logger(), Recovery()
   r := gin.Default()
   // 服务端要给客户端cookie
   r.GET("cookie", func(c *gin.Context) {
      // 获取客户端是否携带cookie
      cookie, err := c.Cookie("key_cookie")
      if err != nil {
         cookie = "NotSet"
         // 给客户端设置cookie
         //  maxAge int, 单位为秒
         // path,cookie所在目录
         // domain string,域名
         //   secure 是否智能通过https访问
         // httpOnly bool  是否允许别人通过js获取自己的cookie
         c.SetCookie("key_cookie", "value_cookie", 60, "/",
            "localhost", false, true)
      }
      fmt.Printf("cookie的值是： %s\n", cookie)
   })
   r.Run(":8000")
}
```

```shell
[root@Docker1 gin]# curl -v http://192.168.1.241:8000/cookie
* About to connect() to 192.168.1.241 port 8000 (#0)
*   Trying 192.168.1.241...
* Connected to 192.168.1.241 (192.168.1.241) port 8000 (#0)
> GET /cookie HTTP/1.1
> User-Agent: curl/7.29.0
> Host: 192.168.1.241:8000
> Accept: */*
>
< HTTP/1.1 200 OK
< Set-Cookie: key_cookie=value_cookie; Path=/; Domain=localhost; Max-Age=60; HttpOnly
< Date: Sun, 20 Mar 2022 16:25:55 GMT
< Content-Length: 0
<
* Connection #0 to host 192.168.1.241 left intact
[root@Docker1 gin]#
```

2. cookie的使用

> 模拟实现权限验证中间件
>
> + 有2个路由，login和home
>
> + login用于设置cookie
>
> + home是访问查看信息的请求
>
> + 在请求home之前，先跑中间件代码，检验是否存在cookie

```go
package main

import (
   "github.com/gin-gonic/gin"
   "net/http"
)

func AuthMiddleWare() gin.HandlerFunc {
   return func(c *gin.Context) {
      // 获取客户端cookie并校验
      if cookie, err := c.Cookie("abc"); err == nil {
         if cookie == "123" {
            c.Next()
            return
         }
      }
      // 返回错误
      c.JSON(http.StatusUnauthorized, gin.H{"error": "err"})
      // 若验证不通过，不再调用后续的函数处理
      c.Abort()
      return
   }
}

func main() {
   // 1.创建路由
   r := gin.Default()
   r.GET("/login", func(c *gin.Context) {
      // 设置cookie
      c.SetCookie("abc", "123", 60, "/",
         "localhost", false, true)
      // 返回信息
      c.String(200, "Login success!")
   })
   r.GET("/home", AuthMiddleWare(), func(c *gin.Context) {
      c.JSON(200, gin.H{"data": "home"})
   })
   r.Run(":8000")
}
```
```shell
[root@Docker1 gin]# curl http://192.168.1.241:8000/login
Login success!
[root@Docker1 gin]# curl http://192.168.1.241:8000/home
{"error":"err"}
[root@Docker1 gin]# curl http://192.168.1.241:8000/home -H "cookie: abc=123;"
{"data":"home"}
```


3. sessions

> gorilla/sessions为自定义session后端提供cookie和文件系统session以及基础结构。
>
> 主要功能是：
>
> - 简单的API：将其用作设置签名（以及可选的加密）cookie的简便方法。
> - 内置的后端可将session存储在cookie或文件系统中。
> - Flash消息：一直持续读取的session值。
> - 切换session持久性（又称“记住我”）和设置其他属性的便捷方法。
> - 旋转身份验证和加密密钥的机制。
> - 每个请求有多个session，即使使用不同的后端也是如此。
> - 自定义session后端的接口和基础结构：可以使用通用API检索并批量保存来自不同商店的session。

```go
package main

import (
    "fmt"
    "net/http"
    "github.com/gorilla/sessions"
)

// 初始化一个cookie存储对象
// something-very-secret应该是一个你自己的密匙，只要不被别人知道就行
var store = sessions.NewCookieStore([]byte("something-very-secret"))

func main() {
    http.HandleFunc("/save", SaveSession)
    http.HandleFunc("/get", GetSession)
    err := http.ListenAndServe(":8080", nil)
    if err != nil {
        fmt.Println("HTTP server failed,err:", err)
        return
    }
}

func SaveSession(w http.ResponseWriter, r *http.Request) {
    // Get a session. We're ignoring the error resulted from decoding an
    // existing session: Get() always returns a session, even if empty.

    //　获取一个session对象，session-name是session的名字
    session, err := store.Get(r, "session-name")
    if err != nil {
        http.Error(w, err.Error(), http.StatusInternalServerError)
        return
    }

    // 在session中存储值
    session.Values["foo"] = "bar"
    session.Values[42] = 43
    // 保存更改
    session.Save(r, w)
}
func GetSession(w http.ResponseWriter, r *http.Request) {
    session, err := store.Get(r, "session-name")
    if err != nil {
        http.Error(w, err.Error(), http.StatusInternalServerError)
        return
    }
    foo := session.Values["foo"]
    fmt.Println(foo)
}
```



```go
    // 删除
    // 将session的最大存储时间设置为小于零的数即为删除
    session.Options.MaxAge = -1
    session.Save(r, w)
```

## 3.7 参数验证

1. 结构体验证

> 用gin框架的数据验证，可以不用解析数据，减少if else，会简洁许多。

```go
//24.go
package main

import (
    "fmt"
    "time"

    "github.com/gin-gonic/gin"
)

//Person ..
type Person struct {
    //不能为空并且大于10
    Age      int       `form:"age" binding:"required,gt=10"`
    Name     string    `form:"name" binding:"required"`
    Birthday time.Time `form:"birthday" time_format:"2006-01-02" time_utc:"1"`
}

func main() {
    r := gin.Default()
    r.GET("/5lmh", func(c *gin.Context) {
        var person Person
        if err := c.ShouldBind(&person); err != nil {
            c.String(500, fmt.Sprint(err))
            return
        }
        c.String(200, fmt.Sprintf("%#v", person))
    })
    r.Run()
}
```

```shell
[root@Docker1 gin]# curl "http://192.168.1.241:8080/5lmh?age=11&name=枯藤&birthday=2006-01-02"
main.Person{Age:11, Name:"枯藤", Birthday:time.Date(2006, time.January, 2, 0, 0, 0, 0, time.UTC)}
[root@Docker1 gin]#
```



+ 自定义验证 validator.v8

示例一：

```go
package main

import (
    "net/http"
    "reflect"
    "github.com/gin-gonic/gin"
    "github.com/gin-gonic/gin/binding"
    "gopkg.in/go-playground/validator.v8"
)

/*
    对绑定解析到结构体上的参数，自定义验证功能
    比如我们要对 name 字段做校验，要不能为空，并且不等于 admin ，类似这种需求，就无法 binding 现成的方法
    需要我们自己验证方法才能实现 官网示例（https://godoc.org/gopkg.in/go-playground/validator.v8#hdr-Custom_Functions）
    这里需要下载引入下 gopkg.in/go-playground/validator.v8
*/
type Person struct {
    Age int `form:"age" binding:"required,gt=10"`
    // 2、在参数 binding 上使用自定义的校验方法函数注册时候的名称
    Name    string `form:"name" binding:"NotNullAndAdmin"`
    Address string `form:"address" binding:"required"`
}

// 1、自定义的校验方法
func nameNotNullAndAdmin(v *validator.Validate, topStruct reflect.Value, currentStructOrField reflect.Value, field reflect.Value, fieldType reflect.Type, fieldKind reflect.Kind, param string) bool {

    if value, ok := field.Interface().(string); ok {
        // 字段不能为空，并且不等于  admin
        return value != "" && !("5lmh" == value)
    }

    return true
}

func main() {
    r := gin.Default()

    // 3、将我们自定义的校验方法注册到 validator中
    if v, ok := binding.Validator.Engine().(*validator.Validate); ok {
        // 这里的 key 和 fn 可以不一样最终在 struct 使用的是 key
        v.RegisterValidation("NotNullAndAdmin", nameNotNullAndAdmin)
    }

    /*
        curl -X GET "http://127.0.0.1:8080/testing?name=&age=12&address=beijing"
        curl -X GET "http://127.0.0.1:8080/testing?name=lmh&age=12&address=beijing"
        curl -X GET "http://127.0.0.1:8080/testing?name=adz&age=12&address=beijing"
    */
    r.GET("/5lmh", func(c *gin.Context) {
        var person Person
        if e := c.ShouldBind(&person); e == nil {
            c.String(http.StatusOK, "%v", person)
        } else {
            c.String(http.StatusOK, "person bind err:%v", e.Error())
        }
    })
    r.Run()
}
```

示例二：

```go
package main

import (
    "net/http"
    "reflect"
    "time"

    "github.com/gin-gonic/gin"
    "github.com/gin-gonic/gin/binding"
    "gopkg.in/go-playground/validator.v8"
)

// Booking contains binded and validated data.
type Booking struct {
    //定义一个预约的时间大于今天的时间
    CheckIn time.Time `form:"check_in" binding:"required,bookabledate" time_format:"2006-01-02"`
    //gtfield=CheckIn退出的时间大于预约的时间
    CheckOut time.Time `form:"check_out" binding:"required,gtfield=CheckIn" time_format:"2006-01-02"`
}

func bookableDate(
    v *validator.Validate, topStruct reflect.Value, currentStructOrField reflect.Value,
    field reflect.Value, fieldType reflect.Type, fieldKind reflect.Kind, param string,
) bool {
    //field.Interface().(time.Time)获取参数值并且转换为时间格式
    if date, ok := field.Interface().(time.Time); ok {
        today := time.Now()
        if today.Unix() > date.Unix() {
            return false
        }
    }
    return true
}

func main() {
    route := gin.Default()
    //注册验证
    if v, ok := binding.Validator.Engine().(*validator.Validate); ok {
        //绑定第一个参数是验证的函数第二个参数是自定义的验证函数
        v.RegisterValidation("bookabledate", bookableDate)
    }

    route.GET("/5lmh", getBookable)
    route.Run()
}

func getBookable(c *gin.Context) {
    var b Booking
    if err := c.ShouldBindWith(&b, binding.Query); err == nil {
        c.JSON(http.StatusOK, gin.H{"message": "Booking dates are valid!"})
    } else {
        c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
    }
}

// curl -X GET "http://localhost:8080/5lmh?check_in=2019-11-07&check_out=2019-11-20"
// curl -X GET "http://localhost:8080/5lmh?check_in=2019-09-07&check_out=2019-11-20"
// curl -X GET "http://localhost:8080/5lmh?check_in=2019-11-07&check_out=2019-11-01"
```



+ 多语言翻译验证

>  gopkg.in/go-playground/validator.v9

```go
package main

import (
    "fmt"

    "github.com/gin-gonic/gin"
    "github.com/go-playground/locales/en"
    "github.com/go-playground/locales/zh"
    "github.com/go-playground/locales/zh_Hant_TW"
    ut "github.com/go-playground/universal-translator"
    "gopkg.in/go-playground/validator.v9"
    en_translations "gopkg.in/go-playground/validator.v9/translations/en"
    zh_translations "gopkg.in/go-playground/validator.v9/translations/zh"
    zh_tw_translations "gopkg.in/go-playground/validator.v9/translations/zh_tw"
)

var (
    Uni      *ut.UniversalTranslator
    Validate *validator.Validate
)

type User struct {
    Username string `form:"user_name" validate:"required"`
    Tagline  string `form:"tag_line" validate:"required,lt=10"`
    Tagline2 string `form:"tag_line2" validate:"required,gt=1"`
}

func main() {
    en := en.New()
    zh := zh.New()
    zh_tw := zh_Hant_TW.New()
    Uni = ut.New(en, zh, zh_tw)
    Validate = validator.New()

    route := gin.Default()
    route.GET("/5lmh", startPage)
    route.POST("/5lmh", startPage)
    route.Run(":8080")
}

func startPage(c *gin.Context) {
    //这部分应放到中间件中
    locale := c.DefaultQuery("locale", "zh")
    trans, _ := Uni.GetTranslator(locale)
    switch locale {
    case "zh":
        zh_translations.RegisterDefaultTranslations(Validate, trans)
        break
    case "en":
        en_translations.RegisterDefaultTranslations(Validate, trans)
        break
    case "zh_tw":
        zh_tw_translations.RegisterDefaultTranslations(Validate, trans)
        break
    default:
        zh_translations.RegisterDefaultTranslations(Validate, trans)
        break
    }

    //自定义错误内容
    Validate.RegisterTranslation("required", trans, func(ut ut.Translator) error {
        return ut.Add("required", "{0} must have a value!", true) // see universal-translator for details
    }, func(ut ut.Translator, fe validator.FieldError) string {
        t, _ := ut.T("required", fe.Field())
        return t
    })

    //这块应该放到公共验证方法中
    user := User{}
    c.ShouldBind(&user)
    fmt.Println(user)
    err := Validate.Struct(user)
    if err != nil {
        errs := err.(validator.ValidationErrors)
        sliceErrs := []string{}
        for _, e := range errs {
            sliceErrs = append(sliceErrs, e.Translate(trans))
        }
        c.String(200, fmt.Sprintf("%#v", sliceErrs))
    }
    c.String(200, fmt.Sprintf("%#v", "user"))
}
```

> http://localhost:8080/testing?user_name=枯藤&tag_line=9&tag_line2=3&locale=en 返回英文的验证信息
>
> http://localhost:8080/testing?user_name=枯藤&tag_line=9&tag_line2=3&locale=zh 返回中文的验证信息

## 3.8 其它

### 3.8.1 日志文件

```go
//25.go
package main

import (
    "io"
    "os"
    "github.com/gin-gonic/gin"
)

func main() {
    gin.DisableConsoleColor()

    // Logging to a file.
    f, _ := os.Create("gin.log")
    gin.DefaultWriter = io.MultiWriter(f)
    // 如果需要同时将日志写入文件和控制台，请使用以下代码。
    // gin.DefaultWriter = io.MultiWriter(f, os.Stdout)
    r := gin.Default()
    r.GET("/ping", func(c *gin.Context) {
        c.String(200, "pong")
    })
    r.Run()
}
```

```shell
[root@Docker1 gin]# cat gin.log
[GIN-debug] [WARNING] Creating an Engine instance with the Logger and Recovery middleware already attached.

[GIN-debug] [WARNING] Running in "debug" mode. Switch to "release" mode in production.
 - using env:   export GIN_MODE=release
 - using code:  gin.SetMode(gin.ReleaseMode)

[GIN-debug] GET    /ping                     --> main.main.func1 (3 handlers)
[GIN-debug] [WARNING] You trusted all proxies, this is NOT safe. We recommend you to set a value.
Please check https://pkg.go.dev/github.com/gin-gonic/gin#readme-don-t-trust-all-proxies for details.
[GIN-debug] Environment variable PORT is undefined. Using port :8080 by default
[GIN-debug] Listening and serving HTTP on :8080
[GIN] 2022/03/25 - 23:07:18 | 404 |         700ns |       127.0.0.1 | GET      "/"
[GIN] 2022/03/25 - 23:07:33 | 200 |      24.599µs |       127.0.0.1 | GET      "/ping"
[root@Docker1 gin]#
```











# 99 学习实例

##  99.1 利用打包资源文件

> fsman.go 

```go
//go version >= 1.6
package main

import (
    "embed"
    "io/fs"
    "log"
    "net/http"
    "os"
)


func main() {
    useOS := len(os.Args) > 1 && os.Args[1] == "live"
    http.Handle("/", http.FileServer(getFileSystem(useOS)))
    http.ListenAndServe(":8888", nil)
}

//go:embed static
var embededFiles embed.FS

func getFileSystem(useOS bool) http.FileSystem {
    if useOS {
        log.Print("using live mode")
        return http.FS(os.DirFS("static"))
    }

    log.Print("using embed mode")

    fsys, err := fs.Sub(embededFiles, "static")
    if err != nil {
        panic(err)
    }
    return http.FS(fsys)
}

```

```shell
[root@localhost golang1111]# find
.
./fsman.go
./static
./static/1.html
./static/abc
./static/abc/2.html
[root@localhost golang1111]#
[root@localhost golang1111]# go build fsman.go
[root@localhost golang1111]# ls
fsman  fsman.go  static
[root@localhost golang1111]# ./fsman &
[1] 1394974
[root@localhost golang1111]# 2022/01/18 23:48:34 using embed mode
[root@localhost golang1111]#
[root@localhost golang1111]# curl http://127.0.0.1:8888/1.html
fdsafdsaf
static/1.htm
[root@localhost golang1111]# curl http://127.0.0.1:8888/
<pre>
<a href="1.html">1.html</a>
<a href="abc/">abc/</a>
</pre>
[root@localhost golang1111]#

```

## 99.2 文件读取

```go
/* 19.go 逐行读取文件 */
package main

import (
    "fmt"
    "bufio"
    "os"
)

func main() {
    ReadLine2("19.txt")
}

func ReadLine2(filename string) {
    f, _ := os.Open(filename)
    defer f.Close()
    r := bufio.NewReader(f)
    for {
        aa, err := readLine(r)
        if err != nil {
            break
        }
        fmt.Println(string(aa))
    }
}

func readLine(r *bufio.Reader) (string, error) {
    line, isprefix, err := r.ReadLine()
    for isprefix && err == nil {
        var bs []byte
        bs, isprefix, err = r.ReadLine()
        line = append(line, bs...)
    }
    return string(line), err
}
```

## 99.3 通用查找，适合各种类型

```go
package main

import (
                "fmt"
                "reflect"
)

//查找字符是否在数组中
func InArray(obj interface{}, target interface{}) (bool) {
        targetValue := reflect.ValueOf(target)
        switch reflect.TypeOf(target).Kind() {
        case reflect.Slice, reflect.Array:
                for i := 0; i < targetValue.Len(); i++ {
                        if targetValue.Index(i).Interface() == obj {
                                return true
                        }
                }
        case reflect.Map:
                if targetValue.MapIndex(reflect.ValueOf(obj)).IsValid() {
                        return true
                }
        }

        return false
}

func main() {
                aaa := "123"
                bbb := []string{"234","34","56","1234"}
                if ! InArray(aaa,bbb) {
                        fmt.Println("hahaha")
                } else {
                        fmt.Println("no hah")
                }
}
```

```shell
[root@5d09d6b9f9d5 golang]# go run  25.go
hahaha
[root@5d09d6b9f9d5 golang]#
```

## 99.4 当前时间

```go
// currentTime.go
package main

import (
                "fmt"
                "time"
           )

func main() {
        timeStr := time.Now().Format("2006-01-02 15:04:05")
        fmt.Println(timeStr)
}
```

```sh 
[root@5d09d6b9f9d5 golang]# go run currentTime.go
2022-03-14 16:10:18
[root@5d09d6b9f9d5 golang]#
```

## 99.5 map2json

```go
//map2json.go
package main

import (
    "encoding/json"
    "fmt"
)

func main() {
    var s =  map[string]interface{}{}
    var a  = map[string]interface{}{"b":11111}

    s["nihao"] = map[string]interface{}{"nihao":"dddd","bb":a}
    res,_ := json.Marshal(s)
    resString := string(res)
    fmt.Println(resString)
}//
```

```shell
[root@5d09d6b9f9d5 golang]# go run map2json.go
{"nihao":{"bb":{"b":11111},"nihao":"dddd"}}
[root@5d09d6b9f9d5 golang]#
```

## 99.6 变量类型

```go
//type_switch.go
package main

import "fmt"

func main() {
         var x interface{}

         switch i := x.(type) {
                case nil:
                 fmt.Println("x 的类型 :%T",i)
                case int:
                 fmt.Println("x 是 int 型")
                case float64:
                 fmt.Println("x 是 float64 型")
                case func(int) float64:
                 fmt.Println("x 是 func(int) 型")
                case bool, string:
                 fmt.Println("x 是 bool 或 string 型" )
                default:
                 fmt.Println("未知型")
         }
}
```

## 99.7 执行shell命令

```go
//testUUID.go
package main

import (
    "fmt"
    "log"
    "os/exec"
)

func main() {
    out, err := exec.Command("uuidgen | sed 's/-//g' | cut -b 1-24").Output()
    if err != nil {
        log.Fatal(err)
    }
    fmt.Printf("%s", out)
}
```

## 99.8 应用程序信息bininfo

```go
//test_bininfo.go
package main

import (
        "flag"
        "fmt"
        "os"
        "BinInfo"
)

func main() {
        v := flag.Bool("v", false, "show bin info")
        flag.Parse()
        if *v {
                _, _ = fmt.Fprint(os.Stderr, BinInfo.StringifyMultiLine())
                os.Exit(1)
        }

        fmt.Println("my app running...")
        fmt.Println("bye...")
}

```

```shell
[root@5d09d6b9f9d5 golang]# cat ./test_bininfo.sh
#!/usr/bin/env bash

#set -x

# 将 log 原始字符串中的单引号替换成双引号
Version="1.0.1"
# 检查源码在git commit 基础上，是否有本地修改，且未提交的内容
Author="Worden"
# 获取当前时间
BuildTime=`date +'%Y-%m-%d %H:%M:%S'`
# 获取 Go 的版本
BuildGoVersion=`go version`

# 将以上变量序列化至 LDFlags 变量中
LDFlags=" \
    -X 'BinInfo.Version=${Version}' \
    -X 'BinInfo.Author=${Author}' \
    -X 'BinInfo.BuildTime=${BuildTime}' \
    -X 'BinInfo.BuildGoVersion=${BuildGoVersion}' \
"

#echo "$LDFlags"

ROOT_DIR=`pwd`

go build -ldflags "$LDFlags" test_bininfo.go

./test_bininfo -v


[root@5d09d6b9f9d5 golang]# ./test_bininfo.sh

test_bininfo Version Info:
        Version: 1.0.1
        Author: Worden
        BuildTime: 2022-03-17 13:27:17
        GoVersion: go version go1.15.14 linux/amd64

[root@5d09d6b9f9d5 golang]#
```

## 99.9 Demo方式（fork）

```go
// mySvr.go
package main

import (
                "os"
                "fmt"
                "time"

)



func main() {
      // when os.Getppid() != 1, not in daemon
           if os.Getppid() != 1 {

                           cmdName := os.Args[0]
            newProc, err := os.StartProcess(cmdName, os.Args, &os.ProcAttr{})
            if err != nil {
                    fmt.Println("Start-Deamon: start process: %s failed, err is: %v", cmdName, err)
                    return
            }

            fmt.Println("Start-Deamon: run in daemon success, pid: %v", newProc.Pid)
            return
        }

        for {
                // will write "test" to file
                fmt.Println("test")
                time.Sleep(time.Duration(3) * time.Second)
        }
}

```

* mySvr.service

```shell
#把文件hxcl.service放到 /usr/lib/systemd/system 目录下 /usr/lib/systemd/system
#命令
#systemctl start hxcl.service 启动
#systemctl stop hxcl.service 停止
#systemctl restart hxcl.service 重启
#systemctl enable hxcl.service 添加为系统自启动服务



#[Unit]部分主要是对这个服务的说明，内容包括Description和After，Description
#用于描述服务，After用于描述服务类别
[Unit]
Description=CBI Service
After=network.service

#[Service]部分是服务的关键，是服务的一些具体运行参数的设置，这里Type=forking
#是后台运行的形式，PIDFile为存放PID的文件路径，ExecStart为服务的具体运行命令，
#ExecReload为重启命令，ExecStop为停止命令，PrivateTmp=True表示给服务分配独
#立的临时空间，注意：[Service]部分的启动、重启、停止命令全部要求使用绝对路径，使
#用相对路径则会报错！
#StandardOutput=null 是将程序业务日志输出到空，也可以指定文件，或者交给journal处理

[Service]
#Type=forking
User=root
Group=root
WorkingDirectory=/home/golang/fork/
ExecStart=cd /home/golang/fork/ && ./mySvr >/dev/null 2>&1 &
#SuccessExitStatus=143
ExecStop=/usr/bin/kill -9 $MAINPID
Environment=HOME=/home/golang/fork/ PWD=/home/golang/fork/
StandardOutput=null
#StandardOutput=/tmp/services/logs/iBot/iBot-run.log

#[Install]部分是服务安装的相关设置，可设置为多用户的
[Install]
WantedBy=multi-user.target
```

## 99.10 动态规划算法

### 99.10.1 动态规划-01背包问题

>有一个背包，容量为4磅，现有如下物品：
>
>| 物品 | 重量（磅） | 价格（元） |
>| ---- | ---------- | ---------- |
>| 吉他 | 1          | 1500       |
>| 音响 | 4          | 3000       |
>| 电脑 | 3          | 2000       |
>
>1 要求达到的目标为装入的背包的总价值最大，并且重量不超出。
>
>2 要求装入的物品不能重复。

```go
//01背包问题--动态规划算法
package main

import (
	"fmt"
)

func main() {

	//待放入背包里的物品价值
	var vGoods = [3]int{1500, 3000, 2000}
	//待放入背包里的物品重量
	var wGoods = [3]int{1, 4, 3}
	//待放入背包容量（重量）
	wBag := 4

	//动态生成一个多维数组
	//按物品数量生成行数，多一行为0
	var vBag = make([][]int, len(wGoods)+1)
	//按物品数量生成行数，多一行为0
	for i := range vBag {
		vBag[i] = make([]int, wBag+1)
	}

	//将第一列置0
	for i := 0; i < len(wGoods)+1; i++ {
		vBag[i][0] = 0
	}
	//将第一行置0
	for j := 0; j < wBag+1; j++ {
		vBag[0][j] = 0
	}

	//i 表示能放入物品的种类
	for i := 1; i < len(wGoods)+1; i++ {
		vGoods1 := vGoods[i-1] //当前种类物品的价值
		wGoods1 := wGoods[i-1] //当前种类物品的重量
		//j 表示背包容量
		for j := 1; j < wBag+1; j++ {
			/*
				如果当前背包重量不能容纳此物品，则取上一个物品当前容量下的价值
				如果能容纳，则max(放入此物品的价值，上一个物品当前容量下的价值)
			*/
			if j < wGoods1 {
				vBag[i][j] = vBag[i-1][j] //上一个物品当前容量下的价值
			} else {
				vTest := vGoods1 + vBag[i-1][j-wGoods1] //假如放入此物品，此时背包里总价值
				if vBag[i-1][j] > vTest {               //比较假如放入此物品的价值与上一个物品当前容量的价值
					vBag[i][j] = vBag[i-1][j] //vBag[i-1][j] -- 上一个物品当前容量下的价值
				} else {
					vBag[i][j] = vTest
				}
			}

		}
	}
	fmt.Println(vBag)
	//背包能够产生的最大总价值
	fmt.Println(vBag[len(wGoods)][wBag])
}

```

### 99.10.2 动态规划-完全背包
>有一个背包，容量为4磅，现有如下物品：
>
>| 物品 | 重量（磅） | 价格（元） |
>| ---- | ---------- | ---------- |
>| 吉他 | 1          | 1500       |
>| 音响 | 4          | 8000       |
>| 电脑 | 3          | 5000       |
>
>1 要求达到的目标为装入的背包的总价值最大，并且重量不超出。
>
>2 每类物品无限多。


```go
//完全背包问题--动态规划算法
package main

import (
	"fmt"
)

func main() {

	//待放入背包里的物品价值
	var vGoods = [3]int{1500, 8000, 5000}
	//待放入背包里的物品重量
	var wGoods = [3]int{1, 4, 3}
	//待放入背包容量（重量）
	wBag := 8

	//动态生成一个多维数组
	//按物品数量生成行数，多一行为0
	var vBag = make([][]int, len(wGoods)+1)
	//按物品数量生成行数，多一行为0
	for i := range vBag {
		vBag[i] = make([]int, wBag+1)
	}

	//将第一列置0
	for i := 0; i < len(wGoods)+1; i++ {
		vBag[i][0] = 0
	}
	//将第一行置0
	for j := 0; j < wBag+1; j++ {
		vBag[0][j] = 0
	}

	//i 表示能放入物品的种类
	for i := 1; i < len(wGoods)+1; i++ {
		vGoods1 := vGoods[i-1] //当前种类物品的价值
		wGoods1 := wGoods[i-1] //当前种类物品的重量
		//j 表示背包容量
		for j := 1; j < wBag+1; j++ {
			vBag[i][j] = vBag[i-1][j]
			//选取放入若干i类物品，取最大价值
			for k := 1; k*wGoods1 <= j; k++ {
				//若放入k个第i类物品，则当前的背包价值=k个物品价值+去掉k个物品重量物品价值
				m := vGoods1*k + vBag[i-1][j-k*wGoods1]
				if vBag[i][j] < m {
					vBag[i][j] = m
				}
			}
		}
	}
	fmt.Println(vBag)
	//背包能够产生的最大总价值
	fmt.Println(vBag[len(wGoods)][wBag])
}

```

### 99.10.3 动态规划-多重背包
>有一个背包，容量为4磅，现有如下物品：
>
>| 物品 | 数量 | 重量（磅） | 价格（元） |
>| ---- | ---- | ---------- | ---------- |
>| 吉他 | 4    | 1          | 1500       |
>| 音响 | 1    | 4          | 8000       |
>| 电脑 | 2    | 3          | 5000       |
>
>要求达到的目标为装入的背包的总价值最大，并且重量不超出。
>

```go
//完全背包问题--动态规划算法
package main

import (
	"fmt"
)

func main() {

	//待放入背包里的物品价值
	var vGoods = [3]int{1500, 8000, 5000}
	//待放入背包里的物品重量
	var wGoods = [3]int{1, 4, 3}
	//待放入背包里的物品数量
	var cGoods = [3]int{4, 1, 2}

	//待放入背包容量（重量）
	wBag := 8

	//动态生成一个多维数组
	//按物品数量生成行数，多一行为0
	var vBag = make([][]int, len(wGoods)+1)
	//按物品数量生成行数，多一行为0
	for i := range vBag {
		vBag[i] = make([]int, wBag+1)
	}

	//将第一列置0
	for i := 0; i < len(wGoods)+1; i++ {
		vBag[i][0] = 0
	}
	//将第一行置0
	for j := 0; j < wBag+1; j++ {
		vBag[0][j] = 0
	}

	//i 表示能放入物品的种类
	for i := 1; i < len(wGoods)+1; i++ {
		vGoods1 := vGoods[i-1] //当前种类物品的价值
		wGoods1 := wGoods[i-1] //当前种类物品的重量
		cGoods1 := cGoods[i-1] //当前种类物品的数量
		//j 表示背包容量
		for j := 1; j < wBag+1; j++ {
			vBag[i][j] = vBag[i-1][j]
			//选取放入若干i类物品，取最大价值
			for k := 1; k <= cGoods1 && k*wGoods1 <= j; k++ {
				//若放入k个第i类物品，则当前的背包价值=k个物品价值+去掉k个物品重量物品价值
				m := vGoods1*k + vBag[i-1][j-k*wGoods1]
				if vBag[i][j] < m {
					vBag[i][j] = m
				}
			}
		}
	}
	fmt.Println(vBag)
	//背包能够产生的最大总价值
	fmt.Println(vBag[len(wGoods)][wBag])
}

```

### 99.10.4 动态规划-数字三角形

> 给字一个由n行数字组成的等腰三角形，试设计一个算法，计算出从三角形的顶至底的一条路径，使该路径经过的数字总和最大。
>
> 输入
>
> 数字三角形的行数和数字三角形
>
> 输出
>
> 最大的路径和以及路径
>
> 输入样例
>
> 5
> 7
> 8 3
> 9 8 7
> 1 2 3 4
> 4 5 6 7 8
>
> 输出样例
>
> 33
> 7 8 8 3 7数字三角形（等腰三角形）。
>
> 本题暂时不考虑路径，只考虑最终结果

#### 解法一、递归做法（自上而下）

```go
//路径最大值--动态规划算法
package main

import (
	"fmt"
)

//函数外，不能使用 := 来声名变量
var maxSumList [5][5]int

func max(i int, j int) int {
	if i > j {
		return i
	} else {
		return j
	}
}

/*
//无记录递归
func MaxSum(i int, j int, n int, D [][]int) int {
	if i >= n-1 {
		return D[i][j]
	}
	x := MaxSum(i+1, j, n, D)
	y := MaxSum(i+1, j+1, n, D)

	fmt.Println(i, j, x, y)
	return max(x, y) + D[i][j]

}
*/

//有记录递归
func MaxSum(i int, j int, n int, D [][]int) int {
	if maxSumList[i][j] != 0 {
		return maxSumList[i][j]
	}
	if i >= n-1 {
		maxSumList[i][j] = D[i][j]
	} else {
		x := MaxSum(i+1, j, n, D)
		y := MaxSum(i+1, j+1, n, D)
		maxSumList[i][j] = max(x, y) + D[i][j]
	}

	return maxSumList[i][j]

}

func main() {
	n := 5
	D := [][]int{{7}, {3, 8}, {8, 1, 0}, {2, 7, 4, 4}, {4, 5, 2, 6, 5}}

	max := MaxSum(0, 0, n, D)

	fmt.Println(max)

}

```

#### 解法二、动态规划（自下而上）

```go
//路径最大值--动态规划算法
package main

import (
	"fmt"
)

func max(i int, j int) int {
	if i > j {
		return i
	} else {
		return j
	}
}

func main() {
	n := 5
	D := [5][5]int{{7}, {3, 8}, {8, 1, 0}, {2, 7, 4, 4}, {4, 5, 2, 6, 5}}
	maxSum := [5][5]int{}

	//最后一层与原值相同
	maxSum[4] = D[4]

	for i := n - 2; i >= 0; i-- {
		for j := 0; j <= i; j++ {
			//上一层的单值，是max(下一层左值 ，下一层右值 ) + 当前值
			maxSum[i][j] = max(maxSum[i+1][j], maxSum[i+1][j+1]) + D[i][j]
		}

	}

	fmt.Println(maxSum[0][0])

	//打印路径
	//列号，初始为0
	l := 0
	fmt.Print(D[0][l], " ")
	for i := 1; i < n; i++ {
		if maxSum[i][l] > maxSum[i][l+1] {
			fmt.Print(D[i][l], " ")
		} else {
			fmt.Print(D[i][l+1], " ")
			l++
		}

	}
	fmt.Println("")

}

```

## 99.11 常用方法

### 99.11.1 二维数组排序

```go
    var nOutList [][]int

	......
	
	//二维数组排序
    sort.Slice(nOutList, func(i,j int) bool {
        nLen := len(nOutList[i])
        if nLen > len(nOutList[j]) {
            nLen = len(nOutList[j])
        }
        for k:=0 ; k<nLen ; k++ {
            if nOutList[i][k] != nOutList[j][k] {
                return nOutList[i][k] < nOutList[j][k] //按顺序排序
            } 
        }
        return true
    })
```

### 99.11.2 二维数组动态生成

```go
    goods := make([][]int, count)
    for i := 0; i < count; i++ {
        goods[i] = make([]int, 3)
    }
```







# Utils 自定义库

## + CommUtil.go ：通用工具

```go
/*
通用工具
1 常用类型
2 常用函数
*/
package Utils

import (
                "fmt"
                seelog "github.com/cihub/seelog"
                "os"
                "io"
                "time"
                "bytes"
                "strings"
                "strconv"
                "reflect"
                "net/http"
                "os/exec"
                "io/ioutil"
                "encoding/json"
)

/* 获取绝对路径 */
func GetRealPath(path string) string {
                if strings.Index(path,"/") == 0  {
                                return path
                } else {
                                pwd,_ := os.Getwd()
                                return pwd+"/"+path
                }
}

/* 字符串转整型 */
func Atoi(s string) int {
                v, err := strconv.Atoi(s)
                if err != nil {
                                seelog.Error("Atoi Error: ",err) ; seelog.Flush()
                                return 0
                }
                return v
}

/* 整型转字符型 */
func Itoa( v int ) string {
                return strconv.Itoa(v)
}


/* 执行shell命令 */
func RunShellCmd(cmd string) string{
                //fmt.Println("Running Shell cmd:" , cmd)
                result, err := exec.Command("/bin/sh", "-c", cmd).Output()
                if err != nil {
                                seelog.Error("Exec Command Error: ",err) ; seelog.Flush()
                }
                return strings.TrimSpace(string(result))
}

/* 判断文件是否存在  存在返回 true 不存在返回false */
func IsFileExist(filename string) bool {
                var exist = true
                if _, err := os.Stat(filename); os.IsNotExist(err) {
                                exist = false
                }
                return exist
}

/* 删除文件 */
func DeleteFile(filename string) bool {
        if IsFileExist(filename) {
                err := os.Remove(filename)
                if err == nil {
                        return true
                }
        }
        return false
}

/* 读取文本文件内容 */
func GetFileContent( filepath string) string {
                bytes, err := ioutil.ReadFile(filepath)

                if err != nil {
                                seelog.Error("read file: ",filepath,", error:", err) ; seelog.Flush()
                                return ""
                }
                return string(bytes)
}

/* 写入文件 */
func WriteFileContent(filepath, content string) {
                var d1 = []byte(content)
                err := ioutil.WriteFile(filepath, d1, 0666) //写入文件(字节数组)
                if err != nil {
                                seelog.Error("Write file: ",filepath,", error:", err) ; seelog.Flush()
                }
}


/* 将字符串转换成二维字符串数组 */
func Str2List(rows string, line_fd string, str_fd string) [][]string {
                rst := [][]string{}
                lines := strings.Split(rows, line_fd)
                for ind := 0 ; ind<len(lines) ;ind++ {
                                rst = append(rst, strings.Split(strings.TrimSpace(lines[ind]), str_fd ))
                }
                return rst
}

/* 打印二维数组 */
func PrintList(rows [][]string ) {
                for ind := 0 ; ind<len(rows) ;ind++ {
                                line :=  rows[ind]
                                for j := 0 ; j <len(line) ; j++ {
                                                fmt.Printf(""+strconv.Itoa(j)+":"+line[j]+"\t")
                                }
                                fmt.Println()
                }
}

/* 从二维数组中查找对应数据 */
func FindListCell(rows [][]string, key string , f_index int, r_index int) string {
                for ind := 0 ; ind<len(rows) ;ind++ {
                                line :=  rows[ind]
                                if len(line) > f_index && len(line) > r_index  {
                                                if line[f_index] == key {
                                                                return line[r_index]
                                                }
                                }
                }
                return ""
}


/* 查找字符是否在数组中 */
func InArray(obj interface{}, target interface{}) (bool) {
                targetValue := reflect.ValueOf(target)
                switch reflect.TypeOf(target).Kind() {
                                case reflect.Slice, reflect.Array:
                                        for i := 0; i < targetValue.Len(); i++ {
                                                if targetValue.Index(i).Interface() == obj {
                                                                return true
                                                }
                                        }
                                case reflect.Map:
                                                if targetValue.MapIndex(reflect.ValueOf(obj)).IsValid() {
                                                                return true
                                }
                }

                return false
}

/* 打印类的所有属性 */
func GetClassAttribute(body interface{}) {
                var prop []string
                refType := reflect.TypeOf(body)
                if refType.Kind() != reflect.Struct {
                                fmt.Println("Not a structure type.")
                }
                for i:=0;i<refType.NumField();i++ {
                                field := refType.Field(i)
                                if field.Anonymous {
                                                prop = append(prop, field.Name)
                                                for j := 0;j<field.Type.NumField();j++ {
                                                                prop = append(prop, field.Type.Field(j).Name)
                                                }
                                                continue
                                }
                                prop = append(prop, field.Name)
                }
                fmt.Printf("%v\n", prop)
}

/* 将Struct转成字符串 */
func Struct2String(v interface{}) string {
                res, _ := json.Marshal(v)
                return string(res)
}


/* 发送GET请求 */
func HttpGet(url string) string {
        // 超时时间：5秒
        client := &http.Client{Timeout: 5 * time.Second}
        resp, err := client.Get(url)
        if err != nil {
                        seelog.Error("HttpGet Error:",err) ; seelog.Flush()
                        return ""
        }
        defer resp.Body.Close()
        var buffer [512]byte
        result := bytes.NewBuffer(nil)
        for {
                n, err := resp.Body.Read(buffer[0:])
                result.Write(buffer[0:n])
                if err != nil && err == io.EOF {
                        break
                } else if err != nil {
                        seelog.Error("HttpGet Error:",err) ; seelog.Flush()
                        return ""
                }
        }

        return result.String()
}

/* 发送POST请求 */
func HttpPost(url string, data string, contentType string) string {
        // 超时时间：5秒
        client := &http.Client{Timeout: 5 * time.Second}
        //jsonStr, _ := json.Marshal(data)
        resp, err := client.Post(url, contentType, strings.NewReader(data))
        if err != nil {
                seelog.Error("HttpPost Error:",err) ; seelog.Flush()
                return ""
        }
        defer resp.Body.Close()

        result, _ := ioutil.ReadAll(resp.Body)
        return string(result)
}
```

## + Gpool.go

```go
package Utils

import (
        "sync"
)

type Pool struct {
        queue chan int
        wg      *sync.WaitGroup
}

func NewPool(size int) *Pool {
        if size <= 0 {
                size = 1
        }
        return &Pool{
                queue: make(chan int, size),
                wg:     &sync.WaitGroup{},
        }
}

func (p *Pool) Add(delta int) {
        for i := 0; i < delta; i++ {
                p.queue <- 1
        }
        for i := 0; i > delta; i-- {
                <-p.queue
        }
        p.wg.Add(delta)
}

func (p *Pool) Done() {
        <-p.queue
        p.wg.Done()
}

func (p *Pool) Wait() {
        p.wg.Wait()
}
```

## + IniUtil.go - ini配置读取

```go
package Utils

import (
        "gopkg.in/ini.v1"
)

type IniParser struct {
        conf_reader *ini.File // config reader
}

type IniParserError struct {
        error_info string
}

func (e *IniParserError) Error() string { return e.error_info }

func (this *IniParser) Load(config_file_name string) error {
        conf, err := ini.Load(config_file_name)
        if err != nil {
                this.conf_reader = nil
                return err
        }
        this.conf_reader = conf
        return nil
}

func (this *IniParser) GetString(section string, key string) string {
        if this.conf_reader == nil {
                return ""
        }

        s := this.conf_reader.Section(section)
        if s == nil {
                return ""
        }

        return s.Key(key).String()
}

func (this *IniParser) GetInt32(section string, key string) int32 {
        if this.conf_reader == nil {
                return 0
        }

        s := this.conf_reader.Section(section)
        if s == nil {
                return 0
        }

        value_int, _ := s.Key(key).Int()

        return int32(value_int)
}

func (this *IniParser) GetUint32(section string, key string) uint32 {
        if this.conf_reader == nil {
                return 0
        }

        s := this.conf_reader.Section(section)
        if s == nil {
                return 0
        }

        value_int, _ := s.Key(key).Uint()

        return uint32(value_int)
}

func (this *IniParser) GetInt64(section string, key string) int64 {
        if this.conf_reader == nil {
                return 0
        }

        s := this.conf_reader.Section(section)
        if s == nil {
                return 0
        }

        value_int, _ := s.Key(key).Int64()
        return value_int
}

func (this *IniParser) GetUint64(section string, key string) uint64 {
        if this.conf_reader == nil {
                return 0
        }

        s := this.conf_reader.Section(section)
        if s == nil {
                return 0
        }

        value_int, _ := s.Key(key).Uint64()
        return value_int
}

func (this *IniParser) GetFloat32(section string, key string) float32 {
        if this.conf_reader == nil {
                return 0
        }

        s := this.conf_reader.Section(section)
        if s == nil {
                return 0
        }

        value_float, _ := s.Key(key).Float64()
        return float32(value_float)
}

func (this *IniParser) GetFloat64(section string, key string) float64 {
        if this.conf_reader == nil {
                return 0
        }

        s := this.conf_reader.Section(section)
        if s == nil {
                return 0
        }

        value_float, _ := s.Key(key).Float64()
        return value_float
}
```

## + SSHUtil.go ：ssh客户端

```go
package Utils

import (
    "net"
    "fmt"
    "bytes"
        "time"
    "golang.org/x/crypto/ssh"
        seelog "github.com/cihub/seelog"
)

type SSHMethod struct {
                session         *ssh.Session
}

//创建ssh链接
func (this *SSHMethod) SSHConnect( user, password, host string, port int ) (bool,error) {
    var (
        auth         []ssh.AuthMethod
        addr         string
        clientConfig *ssh.ClientConfig
        client       *ssh.Client
        session      *ssh.Session
        err          error
    )
    // get auth method
    auth = make([]ssh.AuthMethod, 0)
    auth = append(auth, ssh.Password(password))

    hostKeyCallbk := func(hostname string, remote net.Addr, key ssh.PublicKey) error {
            return nil
    }

    clientConfig = &ssh.ClientConfig{
        User:               user,
        Auth:               auth,
         Timeout:           5 * time.Second,
        HostKeyCallback:    hostKeyCallbk,
    }

    // connet to ssh
    addr = fmt.Sprintf( "%s:%d", host, port )

    if client, err = ssh.Dial( "tcp", addr, clientConfig ); err != nil {
        return false, err
    }

    // create session
    if session, err = client.NewSession(); err != nil {
                seelog.Critical("SSHConnect to ",host," Failed, ",err); seelog.Flush()
        return false,err
    }
        this.session = session
        return true,nil
}

//执行命令并返回结果
func (this *SSHMethod) RunShellCmd(cmd string) string {

        if this.session == nil {
                        seelog.Critical("session is null .") ; seelog.Flush()
                        return ""
        }

    var stdOut, stdErr bytes.Buffer

    this.session.Stdout = &stdOut
    this.session.Stderr = &stdErr

    this.session.Run(cmd)
    return stdOut.String()

}

//关闭链接
func (this *SSHMethod) Close() {
                if this.session != nil {
                                //seelog.Info("Close session") ; seelog.Flush()
                                this.session.Close()
                }
}

```

## + DBUtil.go ：数据库访问类，支持mysql、pg、sqlite

```go
/*
数据库操作类
*/
package Utils

import (
        "fmt"
        "encoding/json"
        _ "github.com/go-sql-driver/mysql" //mysql驱动
        _ "github.com/bmizerany/pq"       //postgresql驱动
        _ "github.com/mattn/go-sqlite3"   //sqlite3驱动
        "github.com/jmoiron/sqlx"
//      "database/sql"
        seelog "github.com/cihub/seelog"
)

//数据库配置参数
type DBMethed struct {
                DBType          string
                Host            string
                Port            string
                Username        string
                Password        string
                Database        string
                DBFile          string
                DBHandle        *sqlx.DB
}



//连接数据库
func (this *DBMethed) ConnDataBase() {
        //默认是mysql数据库
        dbInfo := ""+this.Username+":"+this.Password+"@tcp("+this.Host+":"+this.Port+")/"+this.Database+"" ;
        //postgres 数据库
        if this.DBType == "postgres" {
                dbInfo = fmt.Sprintf("host=%s port=%s user=%s password=%s dbname=%s sslmode=disable",this.Host, this.Port, this.Username, this.Password, this.Database )
        }
        //sqlite3数据库
        if this.DBType == "sqlite3" {
                dbInfo = this.DBFile
        }

        DBHandle, err := sqlx.Open(this.DBType, dbInfo)
        if err != nil {
                seelog.Critical("Open "+this.DBType+" failed,", err) ; seelog.Flush()
                return
        }

        DBHandle.SetMaxOpenConns(200)
        DBHandle.SetMaxIdleConns(20)
        this.DBHandle = DBHandle
}

//关闭数据库连接
func (this *DBMethed) Close() {
        if this.DBHandle != nil {
                this.DBHandle.Close()
        }
}


//查询并转换成Map
func (this *DBMethed) Query(sqlString string)  []map[string]interface{} {
        tableData := make([]map[string]interface{}, 0)
        if this.DBHandle == nil {
                this.ConnDataBase()
        }
        stmt, err := this.DBHandle.Prepare(sqlString)
        if err != nil {
                errinfo := make(map[string]interface{})
                errinfo["ERROR"] = err.Error()
                tableData = append(tableData, errinfo)
                return tableData

        }
        defer stmt.Close()
        rows, err := stmt.Query()
        if err != nil {
                errinfo := make(map[string]interface{})
                errinfo["ERROR"] = err.Error()
                tableData = append(tableData, errinfo)
                return tableData

        }
        defer rows.Close()
        columns, err := rows.Columns()
        if err != nil {
                errinfo := make(map[string]interface{})
                errinfo["ERROR"] = err.Error()
                tableData = append(tableData, errinfo)
                return tableData
        }
        count := len(columns)
        values := make([]interface{}, count)
        valuePtrs := make([]interface{}, count)
        for rows.Next() {
                for i := 0; i < count; i++ {
                        valuePtrs[i] = &values[i]
                }
                rows.Scan(valuePtrs...)
                entry := make(map[string]interface{})
                for i, col := range columns {
                        var v interface{}
                        val := values[i]
                        b, ok := val.([]byte)
                        if ok {
                                v = string(b)
                        } else {
                                v = val
                        }
                        entry[col] = v
                }
                tableData = append(tableData, entry)
        }
        return tableData ;
}

//查询结果转成Json字符串
func (this *DBMethed) Map2Json(tableData []map[string]interface{}, oneFlag bool) string {
        rst := ""
        if oneFlag && len(tableData) > 0 {
                jsonData, err := json.Marshal(tableData[0])
                if err != nil {
                        return "{\"ERROR\":\""+err.Error()+"\"}"
                }
                rst = string(jsonData)
        } else {
                jsonData, err := json.Marshal(tableData)
                if err != nil {
                        return "{\"ERROR\":\""+err.Error()+"\"}"
                }
                rst = string(jsonData)
        }
        return rst
}

//普通查询数据
func (this *DBMethed) Query2Json(sql string) string {
        return this.Map2Json(this.Query(sql), false)

}

func (this *DBMethed) Query2JsonOne(sql string) string {
        return this.Map2Json(this.Query(sql), true)
}

//根据id查询单条数据
func (this *DBMethed) QueryById(TableName, ObjectID string) string {
        sql := "select * from "+TableName+" where id = '"+ObjectID+"' "
        seelog.Debug("Query Sql:", sql) ; seelog.Flush()
        return this.Map2Json(this.Query(sql), true)
}

//查询数据量
func (this *DBMethed) QueryCount(TableName, WhereStr string) string {
        sql := "select count(1) as COUNT from "+TableName+ " where "+WhereStr ;
        seelog.Debug("QueryCount Sql:", sql); seelog.Flush()
        return this.Map2Json(this.Query(sql), true)

}



//执行sql，包括insert,update,delete,create,alter,drop等
func (this *DBMethed) ExecSql(sql string) string {
        if this.DBHandle == nil {
                this.ConnDataBase()
        }
        seelog.Debug("Exec Sql:", sql); seelog.Flush()
        stmt,err := this.DBHandle.Prepare(sql)
        if err != nil {
                return "{\"ERROR\":\""+err.Error()+"\"}"
        }
        rst, err := stmt.Exec()
        if err != nil {
                return "{\"ERROR\":\""+err.Error()+"\"}"
        }
        num, err := rst.RowsAffected()
        if err != nil {
                return "{\"ERROR\":\""+err.Error()+"\"}"
        }
        return "{\"ChageLine\":"+Itoa(num)+"}"
}

//查询表字段，返回字段数组
func (this *DBMethed) QueryFields(TableName string) string {

        if this.DBHandle == nil {
                this.ConnDataBase()
        }

        sqlString := "select * from "+TableName+" where 1 = 2 "

        seelog.Debug(sqlString) ; seelog.Flush();
        stmt, err := this.DBHandle.Prepare(sqlString)
        if err != nil {
                return "{\"ERROR\":\""+err.Error()+"\"}"
        }
        defer stmt.Close()
        rows, err := stmt.Query()
        if err != nil {
                return "{\"ERROR\":\""+err.Error()+"\"}"
        }
        defer rows.Close()
        columns, err := rows.Columns()
        if err != nil {
                return "{\"ERROR\":\""+err.Error()+"\"}"
        }

        var rst map[string][]string
        rst = make(map[string][]string)
        rst["Fields"] = columns

        fmt.Println(rst)
        jsonData, err := json.Marshal(rst)
        if err != nil {
                return "{\"ERROR\":\""+err.Error()+"\"}"
        }
        return string(jsonData)

}
```





# END

