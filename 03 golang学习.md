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





# 学习实例

###  利用打包资源文件

> fsman.go

```go
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

