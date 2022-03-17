# 1 GO基础

## 1.1 基础环境

+ 安装包下载地址为： https://golang.google.cn/dl/
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

#### Type Switch

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

+ 定义结构体

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





## 1.4 切片(Slice)  待补充

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
//多线程并发
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

### 1.8.2 通道（channel）

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

### 1.8.3 遍历通道与关闭通道

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

