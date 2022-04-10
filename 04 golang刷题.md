## 华为OD机试

URL：<https://www.nowcoder.com/exam/oj/ta?page=2&tpId=37&type=37>

###  HJ1 字符串最后一个单词的长度

```go
package main
 
import (
    "fmt"
    "bufio"
    "os"
    "strings"
)
 
func main(){
    inputReader:= bufio.NewReader(os.Stdin)
    input,_,_:=inputReader.ReadLine()
    var str string=string(input)
    str=strings.TrimSpace(str)
    slice:=strings.Split(str," ")
    fmt.Println(len(slice[len(slice)-1]))
}

```

### HJ2 计算某字符出现次数

>写出一个程序，接受一个由字母、数字和空格组成的字符串，和一个字符，然后输出输入字符串中该字符的出现次数。（不区分大小写字母）
>
>数据范围： 1 \le n \le 1000 \1≤*n*≤1000 
>
>输入：
>
>```
>ABCabc
>A
>```
>
>输出：
>
>```
>2
>```

```go
package main

import (
    "os"
    "fmt"
    "bufio"
    "strings"
)

func main() {
    inputReader := bufio.NewReader(os.Stdin)
    input1,_,_:=inputReader.ReadLine()
    var mStr = strings.ToLower( string(input1) )
    input2,_,_ :=inputReader.ReadLine()
    var nStr = strings.ToLower( string(input2) )
    nLen := 0 
    for i:=0 ; i<len(mStr) ; i++ {
        if mStr[i] == nStr[0] {
            nLen++ 
        }
    }
    fmt.Println(nLen)
}
```

### HJ3 明明的随机数

>明明生成了N*N*个1到500之间的随机整数。请你删去其中重复的数字，即相同的数字只保留一个，把其余相同的数去掉，然后再把这些数从小到大排序，按照排好的顺序输出。
>
>数据范围： 1≤*n*≤1000 ，输入的数字大小满足 1≤*v**a**l*≤500 
>
>输入：
>
>```
>3
>2
>2
>1
>```
>
>输出：
>
>```
>1
>2
>```

```go
package main 
import (
    "fmt"
    "os"
    "bufio"
    "strconv"
)

func isNotInList(s [500]int ,n int, a int ) bool {
    for i:=0 ; i < n ; i++ {
        if s[i] == a {
            return false
        }
    }
    return true 
} 


func main() {
    inputReader := bufio.NewReader(os.Stdin)
    nNumStr,_,_ := inputReader.ReadLine() ;
    nNum,_ := strconv.Atoi(string(nNumStr))
    var rst [500]int ;
    nCount := 0 ;
    for i := 0 ; i < nNum ; i++ {
        nNumStr,_,_ = inputReader.ReadLine() ;
        num,_  := strconv.Atoi(string(nNumStr))
        if isNotInList(rst,nCount,num) {
            ind := nCount ;
            for j:=0 ; j<nCount ; j++ {
                if rst[j] > num  {
                    ind = j ;
                    break ;                    
                }
            }
            
            for j:= nCount;j>ind ; j-- {
                rst[j] = rst[j-1]
            }
            rst[ind] = num ;
            nCount++ ;
        }        
    }
    for i:=0 ; i<nCount ; i++ {
        fmt.Println(rst[i])
    }
    
}
```

### HJ4 字符串分隔

> •输入一个字符串，请按长度为8拆分每个输入字符串并进行输出；
>
> •长度不是8整数倍的字符串请在后面补数字0，空字符串不处理。
>
> 输入：
>
> ```
> abc
> ```
>
> 输出：
>
> ```
> abc00000
> ```



```go
package main

import (
	"fmt"
	//   "strings"
    "bufio"
	"os"
)

func main() {
    inputReader := bufio.NewReader(os.Stdin)
    input,_,_ := inputReader.ReadLine() ;
    //input = string(input)
    var ind = 0 
    for i:=0 ;i<len(input) ; i++  {
        fmt.Print(string(input[i]))
        ind++
        if ind%8 == 0 {
            fmt.Println()
            ind=0
        }
    }
    if ind != 0 {
        for i:=ind ; i<8 ; i++ {
           fmt.Print("0") 
        }
        fmt.Println()
    }
}

```

### HJ5 进制转换

>写出一个程序，接受一个十六进制的数，输出该数值的十进制表示。
>
>数据范围：保证结果在 1≤*n*≤231−1 

```go
package main 

import (
    "fmt"
    "bufio"
    "os"
    "strings"
)

func main() {
    inputReader := bufio.NewReader(os.Stdin)
    input,_,_ := inputReader.ReadLine() 

    var rst uint32 
    numMap := map[string]int{
        "0" : 0 ,
        "1" : 1,
        "2" : 2,
        "3" : 3,
        "4" : 4,
        "5" : 5,
        "6" : 6,
        "7" : 7,
        "8" : 8,
        "9" : 9,
        "A" : 10,
        "B" : 11,
        "C" : 12,
        "D" : 13,
        "E" : 14,
        "F" : 15,
    } 
    rst = 0 
    for i:=2 ; i<len(input) ; i++ {
        rst = rst * 16 + uint32( numMap[strings.ToUpper(string(input[i]))] )
    } 
    fmt.Println(rst)
}
```

```go
//别人的答案
package main
 
import "fmt"
 
// 写出一个程序，接受一个十六进制的数，输出该数值的十进制表示。
//
//数据范围：保证结果在 1 \le n \le 2^{31}-1 \1≤n≤2
//31
// −1
 
func main() {
    var num int
    for {
        _, err := fmt.Scanf("0x%x", &num)
        if err != nil {
            return
        }
        fmt.Println(num)
    }
}
```

### HJ6 质数因子

> 功能:输入一个正整数，按照从小到大的顺序输出它的所有质因子（重复的也要列举）（如180的质因子为2 2 3 3 5 ）

```go
package main
import (
    "os"
    "fmt"
    "bufio"
    "strconv"
    //"strings"
)

func main() {
    inputReader := bufio.NewReader(os.Stdin)
    nNumStr,_,_ := inputReader.ReadLine() ;
    nNum,_ := strconv.Atoi(string(nNumStr))

    n := nNum ;
    for i:=2 ; i*i <= nNum ; i++ {
            for n % i == 0  {
                fmt.Print(strconv.Itoa(i) + " ")
                n = n / i
                if n == 1 {
                     goto lend
                }
            }
    }

    if n > 1 {
        fmt.Print(strconv.Itoa(n))
    }
lend:
        fmt.Println()
}

```

### HJ7 取近似值

>写出一个程序，接受一个正浮点数值，输出该数值的近似整数值。如果小数点后数值大于等于 0.5 ,向上取整；小于 0.5 ，则向下取整。

```go
package main
import (
    "os"
    "fmt"
    "bufio"
    "strconv"
    //"strings"
)

func main() {
    inputReader := bufio.NewReader(os.Stdin)
    nNumStr,_,_ := inputReader.ReadLine() ;
    nNum,_ := strconv.ParseFloat(string(nNumStr),32)
    
    rst := int64(nNum + 0.5 )
    fmt.Println(rst)
}
```

```go
package main
import (
    //"os"
    "fmt"
    //"bufio"
    //"strconv"
    //"strings"
)

func main() {
    var nNum float64
    if _,err := fmt.Scan(&nNum) ; err != nil {
        fmt.Printf("Error:%s\n",err)
        return 
    }
    
    rst := int64(nNum + 0.5 )
    fmt.Println(rst)
}
```

### HJ8 合并表记录

> 数据表记录包含表索引index和数值value（int范围的正整数），请对表索引相同的记录进行合并，即将相同索引的数值进行求和运算，输出按照index值升序进行输出。
>
> 提示:
>
> 0 <= index <= 11111111
>
> 1 <= value <= 100000

```go
package main
import (
    "os"
    "fmt"
    "bufio"
    "strconv"
    "strings"
)

func main() {
    inputReader := bufio.NewReader(os.Stdin)
    nNumStr,_,_ := inputReader.ReadLine() ;
    nCount,_ := strconv.Atoi(string(nNumStr))
    
    var nNumMap map[string]int
    nNumMap = make(map[string]int)
    var indexList [500]int
    mCount := 0 ;
    
    for i:= 0 ; i < nCount ; i++ {
        nNumStr,_,_ := inputReader.ReadLine() ;
        nList := strings.Split(string(nNumStr)," ")
        num,ok := nNumMap[nList[0]]
        nIndex , _ :=strconv.Atoi( nList[0] )
        nNum , _ :=strconv.Atoi( nList[1] )
        if ok {
            nNumMap[nList[0]] = num + nNum
        } else {
            nNumMap[nList[0]] = nNum
            j:=0
            for j =0 ; j<mCount ; j++ {
                if indexList[j] > nIndex {
                    break 
                }
            }
            for l := mCount ; l > j ; l-- {
                indexList[l] = indexList[l-1] 
            }
            indexList[j] = nIndex
            mCount++
        }
    }
    for j := 0 ; j<mCount ; j++ {
        nIndex := strconv.Itoa(indexList[j])
        fmt.Println(nIndex+" "+strconv.Itoa(nNumMap[nIndex]))
    }
}
```

```go
//方法二
package main
import (
    "os"
    "fmt"
    "bufio"
    "strconv"
    "strings"
    "sort"
)

func main() {
    inputReader := bufio.NewReader(os.Stdin)
    nNumStr,_,_ := inputReader.ReadLine() ;
    nCount,_ := strconv.Atoi(string(nNumStr))
    
    var nNumMap map[int]int
    nNumMap = make(map[int]int)
    var indexList []int
    
    for i:= 0 ; i < nCount ; i++ {
        nNumStr,_,_ := inputReader.ReadLine() ;
        nList := strings.Split(string(nNumStr)," ")
        nIndex , _ :=strconv.Atoi( nList[0] )
        nNum , _ :=strconv.Atoi( nList[1] )
        nNumMap[nIndex] += nNum
    }
    
    for k := range nNumMap {
        indexList = append(indexList,k)
    }
    //排序
    sort.Ints(indexList)
    for _,v :=range indexList {
        fmt.Println(v,nNumMap[v])
    }
}
```

### HJ9 提取不重复的整数

> 输入一个 int 型整数，按照从右向左的阅读顺序，返回一个不含重复数字的新的整数。
>
> 保证输入的整数最后一位不是 0 。
>
> 数据范围：1≤*n*≤108 

```go
package main
import (
    "os"
    "fmt"
    "bufio"
    "strconv"
    //"strings"
    //"sort"
)

func isNotInList(l []int32, n int32) bool {
    for i := 0 ; i<len(l) ; i++ {
        if l[i] == n {
            return false 
        }
    }
    return true
}

func main() {
    inputReader := bufio.NewReader(os.Stdin)
    nNumStr,_,_ := inputReader.ReadLine() ;
    var nList []int32  
    
    for i := len(nNumStr)-1 ;  i>=0 ; i-- {
        n,_ := strconv.Atoi(string(nNumStr[i]))
        if isNotInList(nList,int32(n)) {
            nList = append(nList,int32(n))
        }
    }
    for _,k := range nList {
        fmt.Print(k)
    }
    fmt.Println()
}

```

### HJ10 字符个数统计

> 编写一个函数，计算字符串中含有的不同字符的个数。字符在 ASCII 码范围内( 0~127 ，包括 0 和 127 )，换行表示结束符，不算在字符里。不在范围内的不作统计。多个相同的字符只计算一次
>
> 例如，对于字符串 abaca 而言，有 a、b、c 三种不同的字符，因此输出 3 。

```go
package main
import (
    "os"
    "fmt"
    "bufio"
    //"strconv"
    //"strings"
    //"sort"
)

func isNotInList(l []byte, n byte) bool {
    for i := 0 ; i<len(l) ; i++ {
        if l[i] == n {
            return false 
        }
    }
    return true
}

func main() {
    inputReader := bufio.NewReader(os.Stdin)
    nStr,_,_ := inputReader.ReadLine() ;
    
    var nList []byte
    var c byte 
    for i:=0 ; i<len(nStr) ; i++ {
        c = nStr[i]
        if isNotInList(nList, c ) {
            nList = append(nList, c )
        }
    }
    fmt.Println(len(nList))
}
```

### HJ12 字符串反转

>接受一个只包含小写字母的字符串，然后输出该字符串反转后的字符串。（字符串长度不超过1000）

```go
package main
import (
    "os"
    "fmt"
    "bufio"
    //"strconv"
    //"strings"
    //"sort"
)

func main() {
    inputReader := bufio.NewReader(os.Stdin)
    nStr,_,_ := inputReader.ReadLine() ;
    
    for i:=len(nStr)-1 ; i>=0 ; i-- {
        fmt.Print(string(nStr[i]))
    }
    fmt.Println()
}
```

### HJ13 句子逆序

> 将一个英文语句以单词为单位逆序排放。例如“I am a boy”，逆序排放后为“boy a am I”
>
> 所有单词之间用一个空格隔开，语句中除了英文字母外，不再包含其他字符

```go
package main
import (
    "os"
    "fmt"
    "bufio"
    //"strconv"
    "strings"
    //"sort"
)

func main() {
    inputReader := bufio.NewReader(os.Stdin)
    nStr,_,_ := inputReader.ReadLine() ;
    
    nStrList := strings.Split(string(nStr) , " ")
    for i:=len(nStrList)-1 ; i>=0 ; i-- {
        fmt.Print(nStrList[i])
        if i > 0 {
            fmt.Print(" ")
        }
    }
    fmt.Println()
}
```

### HJ14 字符串排序

> 给定 n 个字符串，请对 n 个字符串按照字典序排列。
>
> 数据范围： 1≤*n*≤1000 ，字符串长度满足 1≤*l**e**n*≤100 
>
> 输入：
>
> ```
> 9
> cap
> to
> cat
> card
> two
> too
> up
> boat
> boot
> ```
>
> 输出：
>
> ```
> boat
> boot
> cap
> card
> cat
> to
> too
> two
> up
> ```

```go
package main
import (
    "os"
    "fmt"
    "bufio"
    "strconv"
    //"strings"
    "sort"
)

func main() {
    inputReader := bufio.NewReader(os.Stdin)
    nStr,_,_ := inputReader.ReadLine()
    nCount,_ := strconv.Atoi(string(nStr) )
    
    var nStrList []string
    for i := 0 ; i<nCount ; i++ {
        nStr,_,_ := inputReader.ReadLine()
        nStrList = append(nStrList, string(nStr) )
    }
    //字符串排序
    sort.Strings(nStrList)
    
    for _, v := range nStrList {
        fmt.Println(v)
    }
}
```

### HJ15 求int型正整数在内存中存储时1的个数

> 输入一个 int 型的正整数，计算出该 int 型数据在内存中存储时 1 的个数。
>
> 数据范围：保证在 32 位整型数字范围内
>
>  这个数转换成2进制后，输出1的个数

```go
package main
import "fmt"
func main(){
    var input int
    fmt.Scan(&input)
    num:=int32(input)
    res:=0
    n := num 
    for n > 0 {
        if n&1 > 0 {
            res++
        }
        n = n>>1
    }
    fmt.Println(res)
}
```

### HJ16 购物单 (动态规划，未完成）

>王强决定把年终奖用于购物，他把想买的物品分为两类：主件与附件，附件是从属于某个主件的，下表就是一些主件与附件的例子：
>
>| 主件   | 附件           |
>| ------ | -------------- |
>| 电脑   | 打印机，扫描仪 |
>| 书柜   | 图书           |
>| 书桌   | 台灯，文具     |
>| 工作椅 | 无             |
>
>如果要买归类为附件的物品，必须先买该附件所属的主件，且每件物品只能购买一次。
>
>每个主件可以有 0 个、 1 个或 2 个附件。附件不再有从属于自己的附件。
>
>王强查到了每件物品的价格（都是 10 元的整数倍），而他只有 N 元的预算。除此之外，他给每件物品规定了一个重要度，用整数 1 **~** 5 表示。他希望在花费不超过 N 元的前提下，使自己的满意度达到最大。
>
>满意度是指所购买的每件物品的价格与重要度的乘积的总和，假设设第i*i*件物品的价格为v[i]*v*[*i*]，重要度为w[i]*w*[*i*]，共选中了k*k*件物品，编号依次为j_1,j_2,...,j_k*j*1,*j*2,...,*j**k*，则满意度为：v[j_1]*w[j_1]+v[j_2]*w[j_2]+ … +v[j_k]*w[j_k]*v*[*j*1]∗*w*[*j*1]+*v*[*j*2]∗*w*[*j*2]+…+*v*[*j**k*]∗*w*[*j**k*]。（其中 * 为乘号）
>
>请你帮助王强计算可获得的最大的满意度。
>
>输入描述：
>
>输入的第 1 行，为两个正整数N，m，用一个空格隔开：
>
>（其中 N （ N<32000 ）表示总钱数， m （m <60 ）为可购买的物品的个数。）
>
>从第 2 行到第 m+1 行，第 j 行给出了编号为 j-1 的物品的基本数据，每行有 3 个非负整数 v p q
>
>（其中 v 表示该物品的价格（ v<10000 ）， p 表示该物品的重要度（ 1 **~** 5 ）， q 表示该物品是主件还是附件。如果 q=0 ，表示该物品为主件，如果 q>0 ，表示该物品为附件， q 是所属主件的编号）
>
> 输出描述：
>
> 输出一个正整数，为张强可以获得的最大的满意度。

#### 我的代码（错误）

```go
//错误代码
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
	"strings"
	//"sort"
)

type Goods struct {
	index  int
	money  int
	degree int
	gType  int
}

type BuyGoods struct {
	index     int
	allDegree int
	goodsList []int
}

func isInList(nList []int, m int) bool {
	for _, v := range nList {
		if v == m {
			return true
		}
	}
	return false
}

func main() {
	inputReader := bufio.NewReader(os.Stdin)
	nStr, _, _ := inputReader.ReadLine()
	tStr := strings.Split(string(nStr), " ")
	allMoney, _ := strconv.Atoi(tStr[0])
	nCount, _ := strconv.Atoi(tStr[1])

	var nGoodsList []Goods

	for i := 1; i <= nCount; i++ {
		nStr, _, _ := inputReader.ReadLine()
		tStr := strings.Split(string(nStr), " ")
		var nGoods Goods
		nGoods.index = i
		nGoods.money, _ = strconv.Atoi(tStr[0])
		nGoods.degree, _ = strconv.Atoi(tStr[1])
		nGoods.gType, _ = strconv.Atoi(tStr[2])
		nGoodsList = append(nGoodsList, nGoods)
	}

	var nBuyGoodsList []BuyGoods
	var maxAllDegree int

	for i := 0; i < len(nGoodsList); i++ {
		//顺序尝试购买物品
		myiMoney := allMoney
		niAllDegree := 0
		var niBuyGoods BuyGoods
		niBuyGoods.index = nGoodsList[i].index
		if myiMoney >= nGoodsList[i].money {
			if nGoodsList[i].gType == 0 {
				//购买第一个物品
				myiMoney -= nGoodsList[i].money
				niBuyGoods.goodsList = append(niBuyGoods.goodsList, nGoodsList[i].index)
				niAllDegree += nGoodsList[i].money * nGoodsList[i].degree
			} else {
				towMoney := nGoodsList[i].money + nGoodsList[nGoodsList[i].gType-1].money
				if myiMoney >= towMoney {
					//购买附属物品
					myiMoney -= nGoodsList[i].money
					niBuyGoods.goodsList = append(niBuyGoods.goodsList, nGoodsList[i].index)
					niAllDegree += nGoodsList[i].money * nGoodsList[i].degree

					//购买主物品
					myiMoney -= nGoodsList[nGoodsList[i].gType-1].money
					niBuyGoods.goodsList = append(niBuyGoods.goodsList, nGoodsList[i].gType)
					niAllDegree += nGoodsList[nGoodsList[i].gType-1].money * nGoodsList[nGoodsList[i].gType-1].degree

				} else {
					continue
				}
			}
			for k := 0; k < len(nGoodsList)-i-1; k++ {

				nBuyGoods := niBuyGoods
				nAllDegree := niAllDegree
				myMoney := myiMoney
				for j := i + k + 1; j < len(nGoodsList); j++ {
					if isInList(nBuyGoods.goodsList, nGoodsList[j].index) {
						continue
					}
					if myMoney >= nGoodsList[j].money {
						if nGoodsList[j].gType == 0 {
							//如果是主物品，则直接购买
							myMoney -= nGoodsList[j].money
							nBuyGoods.goodsList = append(nBuyGoods.goodsList, nGoodsList[j].index)
							nAllDegree += nGoodsList[j].money * nGoodsList[j].degree

						} else {
							if isInList(nBuyGoods.goodsList, nGoodsList[j].gType) {
								//如果是附属物品，且主物品已经购买
								myMoney -= nGoodsList[j].money
								nBuyGoods.goodsList = append(nBuyGoods.goodsList, nGoodsList[j].index)
								nAllDegree += nGoodsList[j].money * nGoodsList[j].degree

							} else {
								//如果是附属物品，且主物品未购买，则要判断钱是否够买两个物品
								towMoney := nGoodsList[j].money + nGoodsList[nGoodsList[j].gType-1].money
								if myMoney >= towMoney {
									//购买附属物品
									myMoney -= nGoodsList[j].money
									nBuyGoods.goodsList = append(nBuyGoods.goodsList, nGoodsList[j].index)
									nAllDegree += nGoodsList[j].money * nGoodsList[j].degree

									//购买主物品
									myMoney -= nGoodsList[nGoodsList[j].gType-1].money
									nBuyGoods.goodsList = append(nBuyGoods.goodsList, nGoodsList[j].gType)
									nAllDegree += nGoodsList[nGoodsList[j].gType-1].money * nGoodsList[nGoodsList[j].gType-1].degree

								}
							}

						}
					}
				}
                
                nBuyGoods.allDegree = nAllDegree
                nBuyGoodsList = append(nBuyGoodsList, nBuyGoods)
                if maxAllDegree < nAllDegree {
                    maxAllDegree = nAllDegree
                }
			}
		} 

	}
	fmt.Println(maxAllDegree)

}

```

#### 别人的代码

```go
package main 

import (
    "fmt"
)

func main() {
    var money, count int
    fmt.Scan(&money, &count)
    goods := make([][]int, count)
    for i := 0; i < count; i++ {
        goods[i] = make([]int, 3)
    }
    for i := 0; i < count; i++ {
        for j := 0; j < 3; j++ {
            fmt.Scan(&goods[i][j])
        }
    }
    
    costs := make([][]int, 0)
    prices := make([][]int, 0)
    for i := 0; i < count; i++ {
        if  goods[i][2] != 0 {
            continue
        }
        cost := make([]int, 0)
        price := make([]int, 0)
        cost = append(cost, goods[i][0] * goods[i][1])
        price = append(price, goods[i][0])
        for j := 0; j < count; j++ {
            if goods[j][2]-1 == i {
                cost = append(cost, goods[j][0] * goods[j][1] + cost[0])
                price = append(price, goods[j][0] + price[0])
            }
        }
        if len(cost) == 3 {
            cost = append(cost, cost[1] + cost[2] - cost[0])
            price = append(price, price[1] + price[2] - price[0])
        }
        costs = append(costs, cost)
        prices = append(prices, price)
    }
    DpCost(costs, prices, money)
}

func DpCost(cost, price [][]int, money int)  {
    count := len(price)
    packs := make([][]int, count)
    for i := 0; i < count; i++ {
        temp := make([]int, 0)
        for j := 0; j <= money; j++ {
            temp = append(temp, -1)
        }
        packs[i] = temp
    }
    
    packs[0][0] = 0
    len1 := len(price[0])
    for i := 0; i < len1; i++ {
        if price[0][i] <= money {
            packs[0][price[0][i]] = cost[0][i]
        }
    }
    
    for i := 1; i < count; i++ {
        lastPrice := 0
        for j := 0; j <= money; j++ {
            if packs[i-1][j] >= 0 {
                packs[i][j] = packs[i-1][j]
                lastPrice = j
            }
        }

        for j := 0; j <= lastPrice; j++ {
            len2 := len(price[i])
            for k := 0; k < len2; k++ {
                if packs[i-1][j] == -1 {
                    continue
                }
           
                sum := packs[i-1][j] + cost[i][k]
                if j+price[i][k] <= money && sum > packs[i][j+price[i][k]] {
                    packs[i][j+price[i][k]] = sum
                }
            }
        }
       
    }
    
    max := 0
    for i := money; i >= 0; i-- {
        if packs[count-1][i] > 0 && packs[count-1][i] > max {
            max = packs[count-1][i]
        }
    }
    fmt.Println(max)
}
```

### HJ17 坐标移动

> 开发一个坐标计算工具， A表示向左移动，D表示向右移动，W表示向上移动，S表示向下移动。从（0,0）点开始移动，从输入字符串里面读取一些坐标，并将最终输入结果输出到输出文件里面。
>
> 输入：
>
> 合法坐标为A(或者D或者W或者S) + 数字（两位以内）
>
> 坐标之间以;分隔。
>
> 非法坐标点需要进行丢弃。如AA10; A1A; $%$; YAD; 等。
>
> 下面是一个简单的例子 如：
>
> A10;S20;W10;D30;X;A1A;B10A11;;A10;
>
> 处理过程：
>
> 起点（0,0）
>
> \+  A10  = （-10,0）
>
> \+  S20  = (-10,-20)
>
> \+  W10 = (-10,-10)
>
> \+  D30 = (20,-10)
>
> \+  x  = 无效
>
> \+  A1A  = 无效
>
> \+  B10A11  = 无效
>
> \+ 一个空 不影响
>
> \+  A10 = (10,-10)
>
> 结果 （10， -10）

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
	"strings"
	//"sort"
)

func main() {
	inputReader := bufio.NewReader(os.Stdin)
	nStr, _, _ := inputReader.ReadLine()
	nCmdList := strings.Split(string(nStr), ";")
	xAxis := 0
	yAxis := 0
	for _, nCmd := range nCmdList {
        if len(nCmd) < 2 {
            continue 
        }
		nAction := nCmd[0:1]
		nNumStr := nCmd[1:len(nCmd)]
		nNum, err := strconv.Atoi(string(nNumStr))
		if err != nil {
			//转换数据失败，则跳过
			continue
		}

		switch nAction {
		case "A":
			xAxis -= nNum
		case "D":
			xAxis += nNum
		case "W":
			yAxis += nNum
		case "S":
			yAxis -= nNum

		}
	}
    
    fmt.Printf("%d,%d\n",xAxis, yAxis)
}

```

### HJ18 识别有效的IP地址和掩码并进行分类统计

> 请解析IP地址和对应的掩码，进行分类识别。要求按照A/B/C/D/E类地址归类，不合法的地址和掩码单独归类。
>
> 所有的IP地址划分为 A,B,C,D,E五类
>
> A类地址从1.0.0.0到126.255.255.255;
>
> B类地址从128.0.0.0到191.255.255.255;
>
> C类地址从192.0.0.0到223.255.255.255;
>
> D类地址从224.0.0.0到239.255.255.255；
>
> E类地址从240.0.0.0到255.255.255.255
>
> 
>
> 私网IP范围是：
>
> 从10.0.0.0到10.255.255.255
>
> 从172.16.0.0到172.31.255.255
>
> 从192.168.0.0到192.168.255.255
>
> 子网掩码为二进制下前面是连续的1，然后全是0。（例如：255.255.255.32就是一个非法的掩码）
>
> （注意二进制下全是1或者全是0均为非法子网掩码）
>
> 注意：
>
> \1. 类似于【0.*.*.*】和【127.*.*.*】的IP地址不属于上述输入的任意一类，也不属于不合法ip地址，计数时请忽略
>
> \2. 私有IP地址和A,B,C,D,E类地址是不冲突的
>
> 输入描述：
>
> 多行字符串。每行一个IP地址和掩码，用~隔开。
>
> 请参考帖子https://www.nowcoder.com/discuss/276处理循环输入的问题。
>
> 输出描述：
>
> 统计A、B、C、D、E、错误IP地址或错误掩码、私有IP的个数，之间以空格隔开。

```go
package main

import (
	"fmt"
	"strconv"
	"strings"
)

func main() {
	var nStr string
	//记录7个状态值
	var rstList [7]int

	for {
		n, _ := fmt.Scan(&nStr)
		if n == 0 {
			break
		}
		nStrList := strings.Split(nStr, "~")
		if len(nStrList) < 2 {
			continue
		}
		nIPStr := nStrList[0]
		nMaskStr := nStrList[1]

		if strings.HasPrefix(nIPStr, "0.") || strings.HasPrefix(nIPStr, "127.") {
			continue
		}

		if isVaildIP(nIPStr) && isVaildMask(nMaskStr) {
			ipType, isPrivate := checkIPType(nIPStr)
			if ipType == -1 {
				continue
			}
			rstList[ipType]++
			if isPrivate {
				rstList[6]++
			}

		} else {
			rstList[5]++
		}

	}
	//fmt.Println(rstList)
	for _, v := range rstList {
		fmt.Print(v, " ")
	}
	fmt.Println()

}

//判断IP类型
//返回1 int , 0: A类 ; 1: B类 ; 2: C类 ; 3: D类 ; 4: E类 ;-1 :无效
//返回2 bool , 是否是私网地址
func checkIPType(nIPStr string) (int, bool) {
	ipType := -1
	isPrivate := false
	nIPStrList := strings.Split(nIPStr, ".")
	nIPNum_1, _ := strconv.Atoi(nIPStrList[0])
	nIPNum_2, _ := strconv.Atoi(nIPStrList[1])
	//0.*.*.* 127.*.*.* 不属于任意一类
	if nIPNum_1 == 0 || nIPNum_1 == 127 {
		return -1, false
	}
	//判断IP类型
	if nIPNum_1 >= 1 && nIPNum_1 <= 126 {
		ipType = 0
	} else if nIPNum_1 >= 128 && nIPNum_1 <= 191 {
		ipType = 1
	} else if nIPNum_1 >= 192 && nIPNum_1 <= 223 {
		ipType = 2
	} else if nIPNum_1 >= 224 && nIPNum_1 <= 239 {
		ipType = 3
	} else if nIPNum_1 >= 240 && nIPNum_1 <= 255 {
		ipType = 4
	}

	//判断IP是否为私网
	if nIPNum_1 == 10 || (nIPNum_1 == 172 && nIPNum_2 >= 16 && nIPNum_2 <= 31) || (nIPNum_1 == 192 && nIPNum_2 == 168) {
		isPrivate = true
	}

	return ipType, isPrivate
}

//判断IP是否有效IP
func isVaildIP(nIPStr string) bool {
	//判断每段数值在0~255之间
	nIPStrList := strings.Split(nIPStr, ".")
	for _, nIPSection := range nIPStrList {
		mIPSection, err := strconv.Atoi(nIPSection)
		if err != nil {
			return false
		}
		if mIPSection < 0 || mIPSection > 255 {
			return false
		}
	}
	return true
}

//判断掩码是否有效掩码
func isVaildMask(nMaskStr string) bool {

	var nMask uint32
	nMaskStrList := strings.Split(nMaskStr, ".")

	//判断每段数值在0~255之间
	for _, nMaskSection := range nMaskStrList {
		nMaskSection, err := strconv.Atoi(nMaskSection)
		if err != nil {
			return false
		}
		if nMaskSection < 0 || nMaskSection > 255 {
			return false
		}
		nMask = nMask<<8 + uint32(nMaskSection)
	}



	//判断掩码是否合理，前面全是1，后面全是0
	flag := false
	nCount := 0 
	for i := 0; i < 32; i++ {
		if nMask>>i&1 == 1 {
			nCount++
			flag = true
		}
		if flag {
			if nMask>>i&1 == 0 {
				return false
			}
		}

	}
	//二进制下全是1或者全是0均为非法子网掩码
	if nCount == 0 || nCount == 32 {
		return false 
	}

	return true
}

```

### HJ19 简单错误记录

> 开发一个简单错误记录功能小模块，能够记录出错的代码所在的文件名称和行号。
> 处理：
> 1、 记录最多8条错误记录，循环记录，最后只用输出最后出现的八条错误记录。对相同的错误记录只记录一条，但是**错误计数增加。最后一个斜杠后面的带后缀名的部分（保留最后16位）和行号完全匹配的记录才做算是****“****相同****”****的错误记录。**
> 2、 超过16个字符的文件名称，只记录文件的最后有效16个字符；
> 3、 输入的文件可能带路径，记录文件名称不能带路径。**也就是说，哪怕不同路径下的文件，如果它们的名字的后16个字符相同，也被视为相同的错误记录**
> 4、循环记录时，只以第一次出现的顺序为准，后面重复的不会更新它的出现时间，仍以第一次为准
> 数据范围：错误记录数量满足 1 \le n \le 100 \1≤*n*≤100 ，每条记录长度满足 1 \le len \le 100 \1≤*l**e**n*≤100 
> 输入描述：
> 每组只包含一个测试用例。一个测试用例包含一行或多行字符串。每行包括带路径文件名称，行号，以空格隔开。
> 输出描述：
> 将所有的记录统计并将结果输出，格式：文件名 代码行数 数目，一个空格隔开

```go
package main

import (
	"fmt"
	"strings"
)

type ErrStruct struct {
	FileName string
	LineNo   string
	Count    int
}

type ErrMap struct {
	ErrList  []ErrStruct
	ErrCount int
}

func (this *ErrMap) Find(nStr string, nLineNo string) int {

	for index, err := range this.ErrList {
		if err.FileName == nStr && err.LineNo == nLineNo {
			return index
		}

	}
	return -1
}

func (this *ErrMap) Add(nStr string, nLineNo string) {

	//取文件名
	fileNameList := strings.Split(nStr, "\\")
	fileName := fileNameList[len(fileNameList)-1]
	//只记录文件的最后有效16个字符
	if len(fileName) > 16 {
		fileName = fileName[len(fileName)-16:]
	}

	index := this.Find(fileName, nLineNo)

	if index != -1 {
		//有相同记录，则把记录数加1
		this.ErrList[index].Count++
	} else {

		var err ErrStruct
		err.FileName = fileName
		err.LineNo = nLineNo
		err.Count = 1

		/*
			//保持数列里只保留8个元素，但与结果不符合；改为只打印后8条
			if this.ErrCount >= 8 {
				//如果达到8个，则通过移位删除第一个
				for i := 0; i < this.ErrCount-1; i++ {
					this.ErrList[i] = this.ErrList[i+1]
				}
				this.ErrList[this.ErrCount-1] = err

			} else {
				this.ErrList = append(this.ErrList, err)
			}
			this.ErrCount = len(this.ErrList)
		*/

		this.ErrList = append(this.ErrList, err)
		this.ErrCount = len(this.ErrList)

	}

}

func (this *ErrMap) Print() {
	if this.ErrCount < 8 {
		for _, err := range this.ErrList {
			fmt.Printf("%s %s %d\n", err.FileName, err.LineNo, err.Count)
		}

	} else {
		for i := this.ErrCount - 8; i < this.ErrCount; i++ {
			err := this.ErrList[i]
			fmt.Printf("%s %s %d\n", err.FileName, err.LineNo, err.Count)
		}
	}

}

func main() {
	var nStr string
	var nLineNo string

	var errMap ErrMap

	for {
		_, err := fmt.Scanf("%s %s\n", &nStr, &nLineNo)
		if err != nil {
			break
		}
		errMap.Add(nStr, nLineNo)

	}
	errMap.Print()
}

```

### HJ20 密码验证合格程序

> 密码要求:
> 1.长度超过8位
> 2.包括大小写字母.数字.其它符号,以上四种至少三种
> 3.不能有长度大于2的不含公共元素的子串重复 （注：其他符号不含空格或换行）
> 数据范围：输入的字符串长度满足1≤*n*≤100 
> 输入描述：
> 一组字符串。
> 输出描述：
> 如果符合要求输出：OK，否则输出NG

```go
package main

import (
	"fmt"
)

//判断是否有小写
func HaveLower(pwd string) int {
	for i := 0; i < len(pwd); i++ {
		if pwd[i] >= 'a' && pwd[i] <= 'z' {
			return 1
		}
	}
	return 0
}

//判断是否有大写
func HaveUpper(pwd string) int {
	for i := 0; i < len(pwd); i++ {
		if pwd[i] >= 'A' && pwd[i] <= 'Z' {
			return 1
		}
	}
	return 0
}

//判断是否有数字
func HaveNumber(pwd string) int {
	for i := 0; i < len(pwd); i++ {
		if pwd[i] >= '0' && pwd[i] <= '9' {
			return 1
		}
	}
	return 0
}

//判断是否有符号
func HaveSinple(pwd string) int {
	for i := 0; i < len(pwd); i++ {
		if (pwd[i] < '0' || pwd[i] > '9') && (pwd[i] < 'a'  ||  pwd[i] > 'z') && (pwd[i] < 'A'  ||  pwd[i] > 'Z') && pwd[i] != ' ' && pwd[i] != '\n' {
			return 1
		}
	}
	
	return 0
}

func IsRepect(pwd string) bool {
	for i := 0; i < len(pwd)-2; i++ {
		a := pwd[i]
		b := pwd[i+1]
		c := pwd[i+2]
		for j := i + 3; j < len(pwd)-2; j++ {
			if pwd[j] == a && pwd[j+1] == b && pwd[j+2] == c {
				return true
			}
		}
	}
	return false
}

func checkPwd(pwd string) int {
	//判断长度超过8
	if len(pwd) < 8 {
		return 1
	}

	//判断有几种类型
	tCount := HaveLower(pwd) + HaveUpper(pwd) + HaveNumber(pwd) + HaveSinple(pwd)
	if tCount < 3 {
		return 2
	}

	if IsRepect(pwd) {
		return 3
	}
	return -1
}

func main() {
	var nStr string

	for {
		_, err := fmt.Scanf("%s\n", &nStr)
		if err != nil {
			break
		}

		tRst := checkPwd(nStr)
        
		switch tRst {

		case -1:
			fmt.Println("OK")
		default:
			fmt.Println("NG")

		}

	}

}
```

### HJ22 汽水瓶

> 某商店规定：三个空汽水瓶可以换一瓶汽水，允许向老板借空汽水瓶（但是必须要归还）。
>
> 小张手上有n个空汽水瓶，她想知道自己最多可以喝到多少瓶汽水。
>
> 数据范围：输入的正整数满足 1≤*n*≤100 
>
> 注意：本题存在多组输入。输入的 0 表示输入结束，并不用输出结果。
>
> 输入描述：
>
> 输入文件最多包含 10 组测试数据，每个数据占一行，仅包含一个正整数 n（ 1<=n<=100 ），表示小张手上的空汽水瓶数。n=0 表示输入结束，你的程序不应当处理这一行。
>
> 输出描述：
>
> 对于每组测试数据，输出一行，表示最多可以喝的汽水瓶数。如果一瓶也喝不到，输出0。

```go
package main

import (
	"fmt"
)

var etCount int

func switchDrink(count int) {
	//跟老板借一个空瓶，喝完了即归还
	if count == 2 {
		etCount++
	} else {
		
		cntDrink := count / 3
		etCount += cntDrink
		cntLast := count % 3 + cntDrink

		if cntLast >= 2 {
			switchDrink(cntLast)
		}

	}
}

func main() {
	var nCount int

	for {
		_, err := fmt.Scanf("%d\n", &nCount)
		if err != nil {
			break
		}
		if nCount == 0 {
			return
		}

		etCount = 0
		switchDrink(nCount)
		fmt.Println(etCount)

	}
}

```



### HJ24 合唱队(动态规划，未完成)

> N 位同学站成一排，音乐老师要请最少的同学出列，使得剩下的 K 位同学排成合唱队形。
>
> 设K位同学从左到右依次编号为 1，2…，K ，他们的身高分别为T_1,T_2,…,T_K ，若存在*i*(1≤*i*≤*K*) 使得T_1<T_2<......<T_{i-1}<T_i且 T_i>T_{i+1}>......>T_K，则称这K名同学排成了合唱队形。
>
> 通俗来说，能找到一个同学，他的两边的同学身高都依次严格降低的队形就是合唱队形。
>
> 例子： 
>
> 123 124 125 123 121 是一个合唱队形
>
> 123 123 124 122不是合唱队形，因为前两名同学身高相等，不符合要求
>
> 123 122 121 122不是合唱队形，因为找不到一个同学，他的两侧同学身高递减。
>
> 你的任务是，已知所有N位同学的身高，计算最少需要几位同学出列，可以使得剩下的同学排成合唱队形。
>
> **注意：不允许改变队列元素的先后顺序** **且** **不要求最高同学左右人数必须相等**
>
> 数据范围：1≤*n*≤3000 
>
> 输入描述：
>
> 用例两行数据，第一行是同学的总数 N ，第二行是 N 位同学的身高，以空格隔开
>
> 输出描述：
>
> 最少需要几位同学出列

```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
	"strings"
	//"sort"
)

func main() {
	inputReader := bufio.NewReader(os.Stdin)
	nStr, _, _ := inputReader.ReadLine()

	peopleCount, _ := strconv.Atoi(string(nStr))

	nStr, _, _ = inputReader.ReadLine()

	peopleListStr := strings.Split(string(nStr), " ")
	var peopleList []int
	for _, v := range peopleListStr {
		vInt, _ := strconv.Atoi(v)
		peopleList = append(peopleList, vInt)
	}
	// peopleCount := 8
	// //peopleList := []int{186, 186, 150, 200, 160, 130, 197, 200}
	// peopleList := []int{1855, 417, 449, 2562, 1706, 1315, 357, 558, 2275, 1422, 1426, 2582, 1558, 2492, 2067, 145, 2573, 1639, 1409, 2698, 2086, 2809, 1194, 374, 388, 67, 619, 1987, 2241, 1909, 1507, 739, 955, 536, 2161, 1247, 1532, 2943, 479, 657, 2880, 1827, 248, 1290, 60, 183, 1835, 1931, 1646, 1727, 509, 568, 2003, 2899, 1030, 2260, 2357, 2872, 478, 1874, 1561, 1677, 1330, 649, 2148, 1492, 1645, 225, 1103, 51, 2868, 1055, 653, 2186, 2881, 1647, 1819, 2095, 1932, 379, 670, 2496, 2292, 1235, 2671, 2940, 1770, 2927, 360, 1405, 522, 493, 1161, 415, 2254, 2911, 2552, 735, 658, 1856, 1645, 396, 99, 2618, 634, 652, 2238, 117, 2581, 2735, 2330, 1068, 2309, 1623, 790, 2603, 2685, 164, 1548, 1919, 375, 89, 1116, 1077, 896, 1501, 1365, 2611, 2434, 1127, 2328, 927, 1165, 2166, 2120, 18, 1711, 2070, 1176, 495, 2170, 227, 566, 931, 1761, 2099, 699, 662, 1137, 1001, 1661, 1464, 264, 2028, 679, 2128, 1960, 2613, 865, 2513, 863, 1367, 1041, 1522, 2023, 1379, 2518, 528, 1676, 1232, 1624, 1431, 2706, 304, 2226, 2855, 1178, 2908, 2410, 2919, 245, 56, 713, 2384, 874, 1690, 1535, 2892, 70, 1182, 1639, 1236, 1531, 1234, 2401, 2242, 549, 2429, 553, 185, 1370, 2531, 921, 2763, 2391, 544, 2878, 610, 2831, 1868, 12, 1933, 1093, 1818, 698, 2590, 1836, 1433, 2650, 1355, 1933, 2150, 2378, 2222, 1573, 1122, 165, 642, 826, 159, 532, 2935, 1927, 1597, 1687, 2109, 2361, 107, 2033, 20, 915, 1813, 223, 1915, 1071, 737, 2502, 2541, 1496, 437, 2403, 1947, 1088, 914, 1079, 2236, 1469, 2283, 2272, 2245, 2037, 1585, 2637, 2924, 2599, 1382, 1566, 2140, 2782, 1083, 2481, 1371, 1081, 122, 1997, 833, 2448, 139, 2759, 1198, 2346, 1298, 2839, 1363, 2848, 667, 81, 1392, 2662, 1795, 914, 522, 2755, 96, 2873, 2806, 64, 2677, 2326, 1118, 1652, 2531, 2388, 2199, 1047, 2835, 1819, 2018, 2093, 2032, 2936, 1894, 1224, 2457, 2103, 676, 2932, 1119, 553, 2815, 2732, 470, 1904, 452, 1358, 2824, 2784, 1767, 506, 476, 586, 296, 962, 1510, 1684, 1962, 1214, 2760, 2469, 655, 1080, 1601, 1724, 1833, 1700, 2209, 2685, 291, 2694, 1281, 2846, 1752, 2097, 1324, 1807, 19, 1202, 609, 2297, 356, 296, 1802, 236, 1050, 136, 256, 2617, 2142, 2381, 1267, 1950, 467, 2189, 1095, 2000, 1444, 980, 1683, 2524, 2504, 1430, 808, 953, 2289, 2935, 2749, 2613, 1778, 2854, 2614, 2627, 984, 1533, 396, 1941, 1178, 994, 484, 2286, 1610, 2777, 1829, 630, 1050, 2265, 713, 495, 2874, 1595, 1076, 986, 2843, 1428, 1509, 843, 1847, 647, 1769, 2181, 988, 497, 1372, 1739, 1123, 2074, 1432, 1005, 811, 2585, 1799, 2350, 968, 1083, 1052, 1373, 2595, 68, 846, 1506, 1072, 2630, 2884, 867, 1734, 2762, 1383, 840, 2905, 2488, 997, 2283, 2884, 1998, 1642, 1974, 1674, 613, 2199, 635, 1629, 1809, 537, 2350, 2093, 2211, 844, 2258, 2701, 1738, 1780, 2220, 177, 737, 553, 2429, 2270, 818, 844, 724, 2262, 2577, 2751, 2424, 557, 1854, 2931, 2064, 916, 2155, 1858, 337, 1391, 285, 1167, 511, 415, 990, 765, 1856, 2465, 376, 2096, 2697, 2589, 1438, 551, 2026, 2306, 1788, 1192, 111, 1686, 2844, 268, 1374, 2498, 850, 605, 345, 1680, 1725, 1383, 2523, 2639, 1459, 1453, 2904, 2027, 111, 1635, 2082, 1247, 1219, 1536, 2826, 1793, 1101, 1170, 2537, 398, 2869, 1365, 1088, 1032, 1790, 1824, 455, 959, 1089, 1635, 621, 810, 2619, 1882, 45, 1838, 2631, 1837, 233, 2579, 481, 2023, 824, 1397, 1612, 2732, 1761, 1087, 604, 2934, 2863, 0, 1154, 413, 645, 2361, 696, 2255, 1349, 2541, 1524, 1499, 1923, 1699, 2520, 439, 230, 2835, 1290, 261, 1176, 2034, 2054, 2338, 1785, 320, 1636, 26, 2571, 1995, 1698, 2473, 2525, 1053, 2343, 2088, 571, 1042, 2829, 1864, 2884, 1956, 51, 166, 393, 2610, 2524, 2709, 737, 1185, 69, 1360, 493, 2305, 2273, 89, 1442, 2765, 775, 2942, 182, 2092, 1917, 1162, 146, 2310, 1431, 1568, 2486, 1072, 214, 523, 2337, 634, 103, 814, 804, 1817, 527, 2599, 839, 2050, 2238, 503, 1010, 1970, 974, 1541, 698, 1282, 1834, 2038, 1821, 305, 2262, 2261, 1050, 2119, 1832, 1335, 1108, 1305, 1602, 1913, 2510, 1154, 1420, 1310, 1552, 1483, 2517, 762, 1161, 759, 1757, 2853, 2419, 938, 1795, 910, 1987, 2285, 2443, 2293, 2268, 2732, 2266, 619, 410, 612, 404, 2694, 961, 373, 340, 1712, 182, 2213, 2944, 2378, 1908, 1848, 2245, 1577, 2257, 1874, 288, 56, 1982, 2006, 1296, 2479, 918, 33, 1263, 2585, 1804, 2129, 786, 1874, 1373, 232, 2924, 357, 78, 1656, 114, 2861, 501, 533, 374, 1324, 365, 633, 2079, 672, 808, 1736, 2756, 172, 2318, 1486, 2566, 2118, 217, 1655, 1092, 1066, 1443, 400, 583, 2765, 1072, 2495, 2074, 662, 2867, 2789, 64, 188, 2926, 1599, 249, 1880, 247, 2290, 1719, 1970, 594, 558, 1982, 1717, 1195, 913, 2919, 1218, 765, 107, 2627, 2723, 125, 1089, 2726, 2540, 1038, 2902, 1492, 2420, 471, 158, 2891, 1682, 2519, 213, 2780, 1310, 2707, 1988, 856, 859, 2838, 1862, 1342, 1593, 2049, 548, 2435, 403, 1824, 397, 905, 2364, 2226, 1796, 2438, 2827, 419, 1661, 2321, 158, 1815, 2487, 2324, 2268, 126, 2388, 2755, 2778, 1695, 2597, 1496, 2381, 2292, 1895, 827, 577, 1670, 2182, 2040, 2946, 1645, 2838, 1912, 566, 124, 1980, 1000, 1823, 1105, 311, 1535, 2523, 2597, 1545, 2915, 876, 2076, 1708, 2835, 2678, 2438, 2362, 999, 1284, 2171, 77, 2269, 701, 2102, 2266, 2375, 560, 1634, 1831, 1446, 19, 1554, 1927, 947, 1712, 1062, 2441, 1636, 2943, 1368, 872, 33, 789, 2171, 2717, 237, 1570, 388, 789, 2801, 1943, 2515, 2838, 1872, 2676, 2884, 415, 2790, 127, 757, 1187, 1507, 609, 149, 876, 2780, 578, 905, 262, 190, 677, 1878, 2933, 103, 1764, 2490, 1300, 844, 2218, 1938, 2604, 1690, 2830, 2596, 782, 1861, 2100, 1537, 1723, 2381, 2611, 2809, 1498, 2218, 958, 1378, 2782, 1265, 199, 1360, 967, 371, 2500, 665, 2240, 1811, 1535, 2060, 280, 2536, 2228, 589, 448, 1667, 2636, 2076, 750, 1123, 2764, 741, 1500, 1759, 2873, 2, 554, 2523, 108, 2413, 465, 2506, 2279, 2776, 514, 148, 2770, 2452, 1165, 2463, 405, 2208, 2838, 654, 1496, 1690, 1441, 1713, 355, 339, 1009, 1317, 2321, 1737, 866, 2362, 2549, 1255, 1297, 188, 629, 195, 1113, 796, 785, 1716, 1742, 1126, 2565, 968, 666, 879, 1949, 202, 68, 2624, 2001, 1896, 1061, 2072, 736, 1188, 2517, 295, 1228, 1413, 2721, 1528, 2922, 1683, 671, 997, 1133, 138, 1623, 2446, 2524, 44, 43, 2490, 1724, 714, 1720, 82, 767, 292, 1837, 846, 1359, 173, 2738, 1237, 2226, 1140, 2457, 466, 2912, 2343, 872, 1736, 2286, 2935, 2878, 2850, 2139, 1259, 2857, 2786, 2586, 1100, 2149, 1183, 659, 1170, 2481, 789, 823, 2706, 1051, 1300, 321, 2571, 2708, 962, 681, 585, 1376, 2302, 2139, 2156, 1614, 201, 254, 1980, 1890, 626, 1698, 800, 2321, 2586, 884, 2386, 2487, 1557, 1033, 612, 1702, 2628, 529, 1964, 2226, 1688, 1000, 728, 698, 1652, 2871, 151, 1224, 947, 2274, 252, 792, 1981, 1821, 671, 2365, 1111, 1265, 1908, 707, 2252, 219, 1519, 420, 92, 2624, 1194, 2610, 364, 1656, 824, 16, 1139, 1556, 555, 1154, 89, 385, 2387, 2398, 1834, 2313, 2944, 1520, 570, 2034, 2555, 1595, 771, 316, 2084, 2013, 287, 648, 2720, 1835, 1908, 678, 468, 1751, 267, 1767, 1666, 1347, 999, 1114, 1114, 83, 23, 76, 1422, 621, 2513, 2899, 821, 767, 938, 925, 1054, 564, 697, 862, 1513, 2517, 100, 1890, 1775, 946, 480, 708, 1651, 2161, 1403, 2525, 2672, 1251, 2886, 767, 1090, 2310, 1018, 2732, 2214, 759, 631, 524, 2571, 1253, 869, 992, 2255, 2040, 736, 907, 1159, 792, 2741, 1067, 749, 1392, 1789, 441, 1533, 466, 1437, 2223, 543, 2783, 981, 2190, 2390, 317, 535, 162, 2475, 2317, 1302, 1557, 300, 1083, 1214, 964, 500, 1914, 2429, 2625, 2623, 1365, 1765, 2108, 1977, 2719, 1545, 2151, 1283, 1403, 928, 1573, 1042, 833, 425, 315, 854, 1227, 2063, 689, 1121, 2804, 1345, 1380, 2529, 2401, 2803, 2805, 2759, 990, 373, 2768, 2555, 1557, 1644, 2407, 15, 2351, 2791, 1599, 160, 806, 2706, 1600, 331, 932, 1861, 2880, 2695, 1131, 759, 45, 2076, 869, 758, 898, 2423, 702, 1126, 2048, 2380, 2916, 720, 1001, 2211, 983, 2871, 1175, 1714, 2292, 1495, 1417, 1494, 2665, 921, 2868, 1493, 1446, 2084, 1195, 1176, 1970, 437, 2327, 2499, 2943, 871, 30, 2216, 2899, 2155, 2385, 2744, 1359, 1705, 282, 140, 1814, 563, 1134, 343, 1866, 679, 1257, 1385, 2270, 551, 2650, 2754, 1352, 1433, 433, 2208, 2672, 2678, 2448, 740, 2543, 1236, 1923, 2715, 1013, 1232, 1390, 717, 2906, 660, 2796, 521, 1333, 1261, 2430, 1093, 2056, 1166, 982, 636, 803, 676, 2801, 294, 1764, 1629, 118, 2591, 1082, 2736, 1806, 1397, 347, 1017, 2496, 617, 1951, 1512, 2604, 443, 2585, 2460, 2144, 1961, 1115, 1893, 2773, 1793, 2921, 2759, 220, 776, 1075, 1768, 91, 2692, 1321, 1777, 2756, 1498, 394, 894, 1515, 1497, 2293, 750, 1694, 456, 306, 2023, 36, 742, 1692, 1978, 1524, 2183, 888, 2274, 1001, 1067, 1625, 1232, 2191, 42, 828, 2000, 882, 792}
	// peopleCount = len(peopleList)
	// fmt.Println(peopleCount, peopleList)

	//记录第i位的人左侧最多能站几人
	incrList := make([]int, peopleCount)

	//取左侧比i的人矮的j且站在j左侧的人最多的值+1
	for i := 1; i < len(peopleList); i++ {
		maxTemp := 0
		for j := 0; j < i; j++ {
			//如果peopleList[j]比peopleList[i]低，且j的左侧能站的人比0~j的人多，则取 incrList[j]+1
			if peopleList[j] < peopleList[i] && incrList[j]+1 > maxTemp {
				maxTemp = incrList[j] + 1
			}
		}
		incrList[i] = maxTemp
	}

	//记录第i位的人左侧最多能站几人
	deList := make([]int, peopleCount)
	//取右侧比i的人矮的j且站在j右侧的人最多的值+1
	for i := len(peopleList) - 2; i >= 0; i-- {
		maxTemp := 0
		for j := i + 1; j < len(peopleList); j++ {
			//如果peopleList[j]比peopleList[i]低，且j的右侧能站的人比j~len(peopleList)-1的人多，则取 incrList[j]+1
			if peopleList[i] > peopleList[j] && deList[j]+1 > maxTemp {
				maxTemp = deList[j] + 1
			}
		}
		deList[i] = maxTemp
	}

	//合计两侧人数，并且加个自己
	rList := make([]int, peopleCount)
	maxCount := 0 //记录人数最多的记录
	for i := 0; i < len(peopleList); i++ {
		rList[i] = incrList[i] + deList[i] + 1
		//计算最大值，但左右侧至少有一个人
		if incrList[i] != 0 && deList[i] != 0 && rList[i] > maxCount {
			maxCount = rList[i]
		}
	}
	// fmt.Println(incrList)
	// fmt.Println(deList)
	// fmt.Println(rList)

	fmt.Println(peopleCount - maxCount)

}

```

```go
//递归方法，错误，严重超时	
package main

import (
	"fmt"
)

var rmPList []int
var minNum int

func isHlist(plist []int) bool {
	index_mid := 0
	for i := 1; i < len(plist); i++ {
		if plist[index_mid] >= plist[i] {
			break
		}
		index_mid = i
	}
	if index_mid == 0 || index_mid == len(plist)-1 {
		return false
	}
	for i := index_mid + 1; i < len(plist); i++ {
		if plist[index_mid] <= plist[i] {
			break
		}
		index_mid = i
	}
	if index_mid != len(plist)-1 {
		return false
	}
	return true
}

/**
pList 人员列表
n 去掉人员数量
pCount 原有人员总数
*/
func ClacList(pList []int, n int, pCount int) int {

	//fmt.Println(n, pList)

	//至少需要三个人
	if n == pCount-2 {
		return -1
	}
	//比已知最少去除的还要更多，则直接返回
	if n >= minNum {
		return -1
	}
	if isHlist(pList) {
		fmt.Println("isHlist--> ", n, pList)
		rmPList = append(rmPList, n)
		if minNum > n {
			minNum = n
		}
		return 1
	}
	for i := 0; i < len(pList); i++ {
		var a []int
		if i == 0 {
			a = pList[1:]
		} else if i == len(pList)-1 {
			a = pList[0 : len(pList)-1]
		} else {
			a = append(a, pList[:i]...)
			a = append(a, pList[i+1:]...)
		}
		ClacList(a, n+1, pCount)
	}
	return -1
}

func main() {
	peopleCount := 8
	//peopleList := []int{186, 186, 150, 200, 160, 130, 197, 200}
	peopleList := []int{16, 103, 132, 23, 211, 75, 155, 82, 32, 48, 79, 183, 13, 91, 51, 172, 109, 102, 189, 121, 12, 120, 116, 133, 79, 120, 116, 208, 47, 110, 65, 187, 69, 143, 140, 173, 203, 35, 184, 49, 245, 50, 179, 63, 204, 34, 218, 11, 205, 100, 90, 19, 145, 203, 203, 215, 72, 108, 58, 198, 95, 116, 125, 235, 156, 133, 220, 236, 125, 29, 235, 170, 130, 165, 155, 54, 127, 128, 204, 62, 59, 226, 233, 245, 46, 3, 14, 108, 37, 94, 52, 97, 159, 190, 143, 67, 24, 204, 39, 222, 245, 233, 11, 80, 166, 39, 224, 12, 38, 13, 85, 21, 47, 25, 180, 219, 140, 201, 11, 42, 110, 209, 77, 136}
	peopleCount = len(peopleList)
	fmt.Println(peopleCount, peopleList)
	minNum = peopleCount
	ClacList(peopleList, 0, peopleCount)
	fmt.Println(minNum)

}

```

```go
//别人的解题，很正确
package main
 
import "fmt"
 
func main() {
    var n, scanN int
    var heights []int
 
    for {
        scanN, _ = fmt.Scan(&n)
        if scanN == 0 {
            break
        }
        heights = make([]int, n)
        for i := 0; i < n; i++ {
            fmt.Scan(&heights[i])
        }
 
        fmt.Println(chorus(heights))
    }
}
 
// 动态规划
func chorus(heights []int) int {
    // 两个数组分别表示每个人左边与右边最多站的人数
    var leftMost, rightMost = make([]int, len(heights)), make([]int, len(heights))
 
    // 以每个人为中心求解每个人左边最多站的人数
    for center := 1; center < len(heights); center++ {
        // 根据 center 左边已经知晓的每个人的左边最多人数获得 center 左边最多人数
        for i := 0; i < center; i++ {
            if heights[center] > heights[i] && leftMost[center] < leftMost[i]+1 {
                leftMost[center] = leftMost[i] + 1
            }
        }
    }
 
    // 以每个人为中心求解每个人右边最多站的人数
    for center := len(heights) - 2; center >= 0; center-- {
        // 根据 center 右边已经知晓的每个人的右边最多人数获得 center 右边最多人数
        for i := len(heights) - 1; i > center; i-- {
            if heights[center] > heights[i] && rightMost[center] < rightMost[i]+1 {
                rightMost[center] = rightMost[i] + 1
            }
        }
    }
 
    // 获取合唱队的最多人数
    var max = 1
    for i := 0; i < len(heights); i++ {
        if max < leftMost[i]+rightMost[i]+1 {
            max = leftMost[i] + rightMost[i] + 1
        }
    }
 
    return len(heights) - max
}
```



### HJ30 字符串合并处理

> 按照指定规则对输入的字符串进行处理。
>
> 详细描述：
>
> 第一步：将输入的两个字符串str1和str2进行前后合并。如给定字符串 "dec" 和字符串 "fab" ， 合并后生成的字符串为 "decfab"
>
> 第二步：对合并后的字符串进行排序，要求为：下标为奇数的字符和下标为偶数的字符分别从小到大排序。这里的下标的意思是字符在字符串中的位置。注意排序后在新串中仍需要保持原来的奇偶性。例如刚刚得到的字符串“decfab”，分别对下标为偶数的字符'd'、'c'、'a'和下标为奇数的字符'e'、'f'、'b'进行排序（生成 'a'、'c'、'd' 和 'b' 、'e' 、'f'），再依次分别放回原串中的偶数位和奇数位，新字符串变为“abcedf”
>
> 第三步：对排序后的字符串中的'0'~'9'、'A'~'F'和'a'~'f'字符，需要进行转换操作。
>
> 转换规则如下：
>
> 对以上需要进行转换的字符所代表的十六进制用二进制表示并倒序，然后再转换成对应的十六进制大写字符（注：字符 a~f 的十六进制对应十进制的10~15，大写同理）。
>
> 如字符 '4'，其二进制为 0100 ，则翻转后为 0010 ，也就是 2 。转换后的字符为 '2'。
>
> 如字符 ‘7’，其二进制为 0111 ，则翻转后为 1110 ，对应的十进制是14，转换为十六进制的大写字母为 'E'。
>
> 如字符 'C'，代表的十进制是 12 ，其二进制为 1100 ，则翻转后为 0011，也就是3。转换后的字符是 '3'。
>
> 根据这个转换规则，由第二步生成的字符串 “abcedf” 转换后会生成字符串 "5D37BF"。
>
> 数据范围：输入的字符串长度满足 1 \le n \le 100 \1≤*n*≤100 
>
> 输入描述：
>
> 样例输入两个字符串，用空格隔开。
>
> 输出描述：
>
> 输出转化后的结果。
>
> 输入：
>
> ```
> dec fab
> ```
>
> 输出：
>
> ```
> 5D37BF
> ```

```go
package main

import (
	"fmt"
	"sort"
)

func SortStr(nStr string) string {

	var nStr_o []string
	var nStr_e []string

	for i := 0; i < len(nStr); i++ {
		if i%2 == 1 {
			nStr_o = append(nStr_o, string(nStr[i]))
		} else {
			nStr_e = append(nStr_e, string(nStr[i]))
		}
	}
	sort.Strings(nStr_o)
	sort.Strings(nStr_e)
	var nByte []byte
	nByte = make([]byte, len(nStr))
	for i := 0; i < len(nStr); i++ {
		if i%2 == 1 {
			nByte[i] = nStr_o[i/2][0]
		} else {
			nByte[i] = nStr_e[i/2][0]
		}

	}
	return string(nByte)
}

func Transform(nStr string) string {
	var rst []byte
	rst = make([]byte, len(nStr))
	for i := 0; i < len(nStr); i++ {

		a := nStr[i]
		if (a < '0' || a > '9') && (a < 'A' || a > 'F') && (a < 'a' || a > 'f') {
			rst[i] = a
			continue
		}
		var b int
		if a >= '0' && a <= '9' {
			b = int(a - '0')
		} else if a >= 'A' && a <= 'F' {
			b = int(a - 'A' + 10)
		} else if a >= 'a' && a <= 'f' {
			b = int(a - 'a' + 10)
		}

		var c int
		for j := 0; j < 4; j++ {
			c = c<<1 | (b >> j & 1)
		}

		var d byte
		if c >= 0 && c <= 9 {
			d = byte('0' + c)
		} else {
			d = byte('A' + c - 10)
		}

		rst[i] = d

	}
	return string(rst)
}

func main() {
	var aStr string
	var bStr string

	// aStr = "Eqr"
	// bStr = "v9oEb12U2ur4xu7rd931G1f50qDo"

	// nStr := aStr + bStr
	// nStr = SortStr(nStr)
	// fmt.Println(nStr)

	// nStr = Transform(nStr)
	// fmt.Println(nStr)

	for {
		_, err := fmt.Scanf("%s %s\n", &aStr, &bStr)
		if err != nil {
			break
		}
		// aStr = "dec"
		// bStr = "fab"

		nStr := aStr + bStr
		nStr = SortStr(nStr)

		nStr = Transform(nStr)
		fmt.Println(nStr)

	}

}

```

### HJ32 密码截取

> Catcher是MCA国的情报员，他工作时发现敌国会用一些对称的密码进行通信，比如像这些ABBA，ABA，A，123321，但是他们有时会在开始或结束时加入一些无关的字符以防止别国破解。比如进行下列变化 ABBA->12ABBA,ABA->ABAKK,123321->51233214　。因为截获的串太长了，而且存在多种可能的情况（abaaab可看作是aba,或baaab的加密形式），Cathcer的工作量实在是太大了，他只能向电脑高手求助，你能帮Catcher找出最长的有效密码串吗？
>
> 数据范围：字符串长度满足1≤*n*≤2500 
>
> 输入描述：
>
> 输入一个字符串（字符串的长度不超过2500）
>
> 输出描述：
>
> 返回有效密码串的最大长度
>
> 输入：12HHHHA
> 输出：4
>
> 输入：ABBBA
> 输出：5



```go
package main

import (
	"fmt"
	//"sort"
)

func minNum(a int, b int) int {
	if a > b {
		return b
	} else {
		return a
	}
}

func GetMaxLen(nStr string) int {

	maxLen := 0
	for i := 0; i < len(nStr)-1; i++ {
        //偶数对
		if nStr[i] == nStr[i+1] {
			m := 2
			k := minNum(i, len(nStr)-2-i)
			if k > 0 {
				for j := 1; j <= k; j++ {
					if nStr[i-j] == nStr[i+1+j] {
						m += 2
					} else {
						break
					}

				}

			}
			if maxLen < m {
				maxLen = m
			}
		}

		n := 0
		k := minNum(i, len(nStr)-1-i)
		if k > 0 {
            //奇数对
			for j := 1; j <= k; j++ {
				if nStr[i-j] == nStr[i+j] {
					if n == 0 {
						n = 1
					}
					n += 2
				} else {
					break
				}
			}

		}
		if maxLen < n {
			maxLen = n
		}
	}
	fmt.Println(maxLen)
	return maxLen
}

func main() {
	var nStr string
	// nStr = "12HHHHA"
	// GetMaxLen(nStr)

	for {
		_, err := fmt.Scanf("%s\n", &nStr)
		if err != nil {
			break
		}
		GetMaxLen(nStr)
	}

}

```





### HJ33 整数与IP地址间的转换

> 原理：ip地址的每段可以看成是一个0-255的整数，把每段拆分成一个二进制形式组合起来，然后把这个二进制数转变成
> 一个长整数。
> 举例：一个ip地址为10.0.3.193
> 每段数字       相对应的二进制数
> 10          00001010
> 0          00000000
> 3          00000011
> 193         11000001
>
> 组合起来即为：00001010 00000000 00000011 11000001,转换为10进制数就是：167773121，即该IP地址转换后的数字就是它了。
>
> 数据范围：保证输入的是合法的 IP 序列
>
> 输入描述： 
> 1 输入IP地址
> 2 输入10进制型的IP地址
>
> 输出描述：
> 1 输出转换成10进制的IP地址
> 2 输出转换后的IP地址

```go
package main

import (
	"fmt"
	"strconv"
	"strings"
	//"sort"
)

func main() {
	var nStr string
	for {
		_, err := fmt.Scanf("%s\n", &nStr)
		if err != nil {
			break
		}

		ipNum, err := strconv.Atoi(nStr)

		if err == nil {
			var ipStrList []string
			ipStrList = make([]string, 4)
			for i := 3; i > -1; i-- {
				ipStrList[i] = strconv.Itoa(ipNum & 255)
				ipNum = ipNum >> 8
			}
			fmt.Println(strings.Join(ipStrList, "."))

		} else {
			ipNum := 0
			ips := strings.Split(string(nStr), ".")
			for _, ipSec := range ips {
				ipSecNum, _ := strconv.Atoi(ipSec)
				ipNum = ipNum<<8 + ipSecNum
			}
			fmt.Println(ipNum)
		}
	}

}

```

### HJ36 字符串加密

> 有一种技巧可以对数据进行加密，它使用一个单词作为它的密匙。下面是它的工作原理：首先，选择一个单词作为密匙，如TRAILBLAZERS。如果单词中包含有重复的字母，只保留第1个，将所得结果作为新字母表开头，并将新建立的字母表中未出现的字母按照正常字母表顺序加入新字母表。如下所示：
>
> A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
>
> T R A I L B Z E S C D F G H J K M N O P Q U V W X Y (实际需建立小写字母的字母表，此字母表仅为方便演示）
>
> 上面其他用字母表中剩余的字母填充完整。在对信息进行加密时，信息中的每个字母被固定于顶上那行，并用下面那行的对应字母一一取代原文的字母(字母字符的大小写状态应该保留)。因此，使用这个密匙， Attack AT DAWN (黎明时攻击)就会被加密为Tpptad TP ITVH。
>
> 请实现下述接口，通过指定的密匙和明文得到密文。
>
> 数据范围：1≤*n*≤100 ，保证输入的字符串中仅包含小写字母
>
> 输入描述：
>
> 先输入key和要加密的字符串
>
> 输出描述：
>
> 返回加密后的字符串

```go
package main

import (
	"fmt"
	"strings"
	//"sort"
)

//生成新字典
func MakeAlphabet(nKey string) map[string]string {
	var rst map[string]string
	rst = make(map[string]string) //集合使用需要先make生成
	//以字母表顺序生成数组
	var alphabetList = []string{"a", "b", "c", "d", "e", "f", "g", "h", "i", "j", "k", "l", "m", "n", "o", "p", "q", "r", "s", "t", "u", "v", "w", "x", "y", "z"}

	//先将key串内的字符写入集合
	for i := 0; i < len(nKey); i++ {
		rst[alphabetList[i]] = string(nKey[i])
	}
	//将其它字符顺序写入
	for i := len(nKey); i < len(alphabetList); i++ {
		var v string
		//顺序查找未写入的字母
		for j := 0; j < len(alphabetList); j++ {
			v = alphabetList[j]
			//是否已经存储过
			flag := 0
			for _, alphabet := range rst {
				if alphabetList[j] == alphabet {
					flag = 1
				}
			}
			if flag == 0 {
				break
			}
		}
		rst[alphabetList[i]] = v
	}
	return rst
}

//去除字符串中重复字符
func DuplicateRemove(nKey string) string {
	var n []byte
	for i := 0; i < len(nKey); i++ {
		flag := 0
		for j := 0; j < len(n); j++ {
			if nKey[i] == n[j] {
				flag = 1
				break
			}
		}
		if flag == 0 {
			n = append(n, nKey[i])
		}

	}
	return string(n)
}

//加密字符串
func EncryptString(nKey string, nStr string) string {
	var rst string
	nKey_low := strings.ToLower(nKey)

	//去重，产生新的字典，字典为小写
	nAlphabeMap := MakeAlphabet(DuplicateRemove(nKey_low))

	var rstList []string
	for i := 0; i < len(nStr); i++ {
		if nStr[i] >= 'A' && nStr[i] <= 'Z' { //处理大写字母，先转换成小写，再转换成大写
			rstList = append(rstList, strings.ToUpper(nAlphabeMap[strings.ToLower(string(nStr[i]))]))
		} else if nStr[i] >= 'a' && nStr[i] <= 'z' {
			rstList = append(rstList, nAlphabeMap[string(nStr[i])])
		} else { //其它字符直接写入
			rstList = append(rstList, string(nStr[i]))
		}
	}
	rst = strings.Join(rstList, "")

	return rst
}

func main() {
	var nStr string
	var nKey string

	// nKey = "TRAILBLAZERS"
	// nStr = "Attack AT DAWN"
	// rst := EncryptString(nKey, nStr)
	// fmt.Println(rst)

	for {
		_, err := fmt.Scanf("%s\n", &nKey)
		if err != nil {
			break
		}
		_, err = fmt.Scanf("%s\n", &nStr)
		if err != nil {
			break
		}
		rst := EncryptString(nKey, nStr)
		fmt.Println(rst)
	}

}

```



### HJ38 求小球落地5次后所经历的路程和第5次反弹的高度

```go
package main

import (
	"fmt"
)

func main() {
	var oHight float32
	_, err := fmt.Scanf("%f\n", &oHight)
	if err != nil {
		return
	}

	n := 5
	iHigh := oHight
	var allCount float32
	for i := 0; i < n; i++ {
		allCount = iHigh * 2
		iHigh = iHigh / 2

	}
	allCount -= oHight
	fmt.Printf("%.6f\n%.6f\n", allCount, iHigh)
}

```

### HJ41 称砝码

> 现有n种砝码，重量互不相等，分别为 m1,m2,m3…mn ；
> 每种砝码对应的数量为 x1,x2,x3...xn 。现在要用这些砝码去称物体的重量(放在同一侧)，问能称出多少种不同的重量。
>
> 注：称重重量包括 0
>
> 输入：
>
> 对于每组测试数据：
> 第一行：n --- 砝码的种数(范围[1,10])
> 第二行：m1 m2 m3 ... mn --- 每种砝码的重量(范围[1,2000])
> 第三行：x1 x2 x3 .... xn --- 每种砝码对应的数量(范围[1,10])
>
> 输出：
>
> 利用给定的砝码可以称出的不同的重量数

```go
package main

import (
	"fmt"
)

//判断元素是否在数组内
func isInList(list []int, n int) bool {
	for _, v := range list {
		if v == n {
			return true
		}
	}
	return false
}

func main() {
	var n int
	_, err := fmt.Scanf("%d", &n)
	if err != nil {
		return
	}

	//组成质量数组
	var mList []int
	for i := 0; i < n; i++ {
		var m int
		_, err := fmt.Scanf("%d", &m)
		if err != nil {
			return
		}
		mList = append(mList, m)
	}

	//组成数量数组
	var xList []int
	for i := 0; i < n; i++ {
		var m int
		_, err := fmt.Scanf("%d", &m)
		if err != nil {
			return
		}
		xList = append(xList, m)
	}
	//fmt.Println(n, mList, xList)

	//存放计算结果，不重复
	var xmList []int
	//放入0值
	xmList = append(xmList, 0)
	for i := 0; i < n; i++ {
		//第i个砝码的数量
		k := xList[i]
		for j := 1; j <= k; j++ {
			//记录结果数组在计算第j个i前有多少个元素
			l := len(xmList)
			//对原有结果，逐一加上i的重量，判断是否重复，如果不重复则加入结果集
			for o := 0; o < l; o++ {
				s := xmList[o] + mList[i]
				if !isInList(xmList, s) {
					xmList = append(xmList, s)
				}
			}
		}

	}

	//fmt.Println(xmList)
	fmt.Println(len(xmList))

}

```

### HJ42 学英语

> Jessi初学英语，为了快速读出一串数字，编写程序将数字转换成英文：
>
> 具体规则如下:
> 1.在英语读法中三位数字看成一整体，后面再加一个计数单位。从最右边往左数，三位一单位，例如12,345 等
> 2.每三位数后记得带上计数单位 分别是thousand, million, billion.
> 3.公式：百万以下千以上的数 X thousand X, 10亿以下百万以上的数：X million X thousand X, 10 亿以上的数：X billion X million X thousand X. 每个X分别代表三位数或两位数或一位数。
> 4.在英式英语中百位数和十位数之间要加and，美式英语中则会省略，我们这个题目采用加上and，百分位为零的话，这道题目我们省略and
>
> 下面再看几个数字例句：
>
> 22: twenty two
>
> 100: one **hundred**
>
> 145: one **hundred** **and** forty five
> 1,234: one **thousand** two **hundred** **and** thirty four
> 8,088: eight **thousand** (and) eighty eight (注:这个and可加可不加，这个题目我们选择不加)
> 486,669: four **hundred** **and** eighty six **thousand** six hundred **and** sixty nine
> 1,652,510: one **million** six **hundred** **and** fifty two **thousand** five **hundred** **and** ten
>
> 说明：
> 数字为正整数，不考虑小数，转化结果为英文小写；
> 保证输入的数据合法
>
> 关键字提示：and，billion，million，thousand，hundred。
>
> 数据范围：1≤*n*≤2000000 
>
> 输入描述：
>
> 输入一个long型整数
>
> 输出描述：
>
> 输出相应的英文写法

```go
package main

import "fmt"

//获取千以内的英文
func GetEnglishHundredNum(n int, andFlag bool) string {

	var EnglishNumList_1 = []string{"one", "two", "three", "four", "five", "six", "seven", "eight", "nine"}
	var EnglishNumList_2 = []string{"eleven", "twelve", "thirteen", "fourteen", "fifteen", "sixteen", "seventeen", "eighteen", "nineteen"}
	var EnglishNumList_3 = []string{"ten", "twenty", "thirty", "forty", "fifty", "sixty", "seventy", "eighty", "ninety"}

	rst := ""
	if n >= 100 {
		rst = EnglishNumList_1[n/100-1] + " hundred"
		n = n % 100
		if n > 0 {
			if andFlag {
				rst += " and"
			}
		}
	}
	if n >= 20 {
		if rst == "" {
			rst = EnglishNumList_3[n/10-1]
		} else {
			rst += " " + EnglishNumList_3[n/10-1]
		}
		n = n % 10
	} else if n == 10 {
		if rst != "" {
			rst += " "
		}
		rst += "ten"
		n = 0
	} else if n > 10 {
		if rst != "" {
			rst += " "
		}
		rst +=  EnglishNumList_2[n-11]
		n = 0
	}

	if n > 0 {
		if rst == "" {
			rst = EnglishNumList_1[n-1]
		} else {
			rst += " " + EnglishNumList_1[n-1]
		}
	}
	return rst
}

//按进制进行数据n
func GetEnglishNum(n int, andFlag bool) string {
	rst := ""
	if n > 999 {
		andFlag = true
	}
	if n/1000000000 > 0 {
		rst += GetEnglishHundredNum(n/1000000000, andFlag) + " billion"
		n = n % 1000000000
	}
	if n/1000000 > 0 {
		if rst != "" {
			rst += " "
		}
		rst += GetEnglishHundredNum(n/1000000, andFlag) + " million"
		n = n % 1000000
	}
	if n/1000 > 0 {
		if rst != "" {
			rst += " "
		}
		rst += GetEnglishHundredNum(n/1000, andFlag) + " thousand"
		n = n % 1000
	}
	if n > 0 {
		if rst != "" {
			rst += " "
		}
		rst += GetEnglishHundredNum(n, andFlag)
		n = n % 1000
	}
	return rst
}

func main() {
	var n int
	_, err := fmt.Scanf("%d", &n)
	if err != nil {
		return
	}

	fmt.Println( GetEnglishNum(n, false))

}
```

### HJ48 从单向链表中删除指定值的节点

> 输入一个单向链表和一个节点的值，从单向链表中删除等于该值的节点，删除后如果链表中无节点则返回空指针。
>
> 链表的值不能重复。
>
> 构造过程，例如输入一行数据为:
>
> 6 2 **1 2** **3 2** **5 1** **4 5** **7 2** 2
>
> 则第一个参数6表示输入总共6个节点，第二个参数2表示头节点值为2，剩下的2个一组表示第2个节点值后面插入第1个节点值，为以下表示:
>
> 1 2 表示为
>
> 2->1
>
> 链表为2->1
>
> 3 2表示为
>
> 2->3
>
> 链表为2->3->1
>
> 5 1表示为
>
> 1->5
>
> 链表为2->3->1->5
>
> 4 5表示为
>
> 5->4
>
> 链表为2->3->1->5->4
>
> 7 2表示为
>
> 2->7
>
> 链表为2->7->3->1->5->4
>
> 最后的链表的顺序为 2 7 3 1 5 4
>
> 最后一个参数为2，表示要删掉节点为2的值
>
> 删除 结点 2
>
> 则结果为 7 3 1 5 4

```go
package main
import (
    "fmt"
)

type node struct {
    num int
    next *node
}

//插入新的节点
func InsertNode(pHeader *node , iNum int , aNum int) {
    var nNode node
    nNode.num = iNum
    nNode.next = nil
    for i:=pHeader ; i!=nil; i = i.next {
        if i.num == aNum {
            nNode.next=i.next
            i.next = &nNode
            break
        }
        
    }
}

func DeleteNode(pHeader *node , iNum int) {
    j:=pHeader
    for i:=pHeader ; i!=nil; i = i.next {
        if i.num == iNum {
            if i == pHeader {
                pHeader = i.next
            } else {
                j.next = i.next
            }
            break
        }
        j = i 
    }    
}

func PrintNode(pHeader *node) {
    for i:=pHeader ; i!=nil; i = i.next {
        fmt.Print(i.num)
        if i.next != nil {
            fmt.Print(" ")
        }
    }
    fmt.Println()
}

func main() {
    var nCount int
    var nbr1,nbr2 int 
    var pHeader *node
    _,err := fmt.Scanf("%d",&nCount)
    if err != nil {
        return 
    }
    //读取第一节点数字
    _,err = fmt.Scanf("%d",&nbr1)
    if err != nil {
        return 
    }
    var nNode node
    pHeader = &nNode
    nNode.num = nbr1
    nNode.next = nil
    //读取后续节点数字
    for i:=0 ; i<nCount-1;i++ {
        _,err = fmt.Scanf("%d %d",&nbr1,&nbr2)
        if err != nil {
            return 
        }
        InsertNode(pHeader,nbr1, nbr2)
    }
    
    //删除节点
    _,err = fmt.Scanf("%d",&nbr1)
    if err != nil {
        return 
    }
    DeleteNode(pHeader, nbr1)
    //打印链接
    PrintNode(pHeader)
    
}
```

### HJ57 高精度整数加法

> 输入两个用字符串 str 表示的整数，求它们所表示的数之和。

```go
package main 

import (
    "fmt"
    "math/big"
)

func main() {
    var a,b string 
    fmt.Scan(&a,&b)
    num1 :=new(big.Int)
    num2 :=new(big.Int)
    
    num1.SetString(a,10)
    num2.SetString(b,10)
    
    num1.Add(num1,num2)
    fmt.Println(num1.String())
}

```





### HJ67 24点游戏算法

> 给出4个1-10的数字，通过加减乘除运算，得到数字为24就算胜利,除法指实数除法运算,运算符仅允许出现在两个数字之间,本题对数字选取顺序无要求，但每个数字仅允许使用一次，且需考虑括号运算
>
> 此题允许数字重复，如3 3 4 4为合法输入，此输入一共有两个3，但是每个数字只允许使用一次，则运算过程中两个3都被选取并进行对应的计算操作。
>
> 输入描述：
>
> 读入4个[1,10]的整数，数字允许重复，测试用例保证无异常数字。
>
> 输出描述：
>
> 对于每组案例，输出一行表示能否得到24点，能输出true，不能输出false
>
> 输入：7 2 1 10
>
> 输出：true

```go
package main

import (
    "fmt"
)

//四则运算，1：+；2：-；3：* ；4：/ ; 
//返回 -1 ：不正确
func CalcNum(n1 int, n2 int, f int) int {
    rst := -1
    switch f {
    case 1 :
        rst = n1 + n2 
    case 2 :
        rst = n1 - n2
    case 3 :
        rst = n1 * n2
    case 4 : 
        if n1 >= n2 && n1%n2==0 {
            rst = n1 / n2
        } 
            
    }
    return rst
}

var visited []bool

func Calc24Point(list []int , num int , sum int ) bool {
    if sum < 0 {
        return false 
    }
    if num== 4 && sum == 24 {
        return true
    }
    
    for  i:=0 ; i<4; i++ {
        if ! visited[i] {
            visited[i]=true
            if Calc24Point(list,num+1,CalcNum(sum ,list[i] , 1) ) || Calc24Point(list,num+1,CalcNum(sum ,list[i] , 2) ) ||Calc24Point(list,num+1,CalcNum(sum ,list[i] , 3) ) ||Calc24Point(list,num+1,CalcNum(sum ,list[i] , 4) ) {
                return true
            }
            visited[i]=false
        }
    }
    return false 
    
}

func main() {
    var list []int
    var num int
    
    for i:=0 ; i<4; i++ {
        _,err := fmt.Scan(&num)
        if err != nil {
            return 
        }
        visited = append(visited , false)
        list = append(list, num)
    }
    rst := Calc24Point(list,0,0)
    fmt.Println(rst)
}

```

### HJ77 火车进站(有错误)

> 给定一个正整数N代表火车数量，0<N<10，接下来输入火车入站的序列，一共N辆火车，每辆火车以数字1-9编号，火车站只有一个方向进出，同时停靠在火车站的列车中，只有后进站的出站了，先进站的才能出站。
>
> 要求输出所有火车出站的方案，以字典序排序输出。
>
> 数据范围：1\le n\le 10\1≤*n*≤10 
>
> 进阶：时间复杂度：O(n!)\*O*(*n*!) ，空间复杂度：O(n)\*O*(*n*) 
>
> 输入描述：
>
> 第一行输入一个正整数N（0 < N <= 10），第二行包括N个正整数，范围为1到10。
>
> 输出描述：
>
> 输出以字典序从小到大排序的火车出站序列号，每个编号以空格隔开，每个输出序列换行，具体见sample。
>
> 输入：
> 3
> 1 2 3
>
> 输出：
> 1 2 3
> 1 3 2
> 2 1 3
> 2 3 1
> 3 2 1
>
> 说明：
> 第一种方案：1进、1出、2进、2出、3进、3出
> 第二种方案：1进、1出、2进、3进、3出、2出
> 第三种方案：1进、2进、2出、1出、3进、3出
> 第四种方案：1进、2进、2出、3进、3出、1出
> 第五种方案：1进、2进、3进、3出、2出、1出
> 请注意，[3,1,2]这个序列是不可能实现的。    



```go
package main

import (
	"fmt"
)

//结果集
var nOutList [][]int

func TrainRun(inList []int, outList []int, nOut []int, index int) {
	fmt.Println(index, "---> ", inList, outList, nOut)
	if len(inList) == 0 && len(outList) == 0 {
		nOutList = append(nOutList, nOut)
		return
	}
	//有待出站的车，就先出站
	if len(outList) > 0 {
		var temp_in, temp_out, temp_nOut []int
		if len(inList) > 0 {
			temp_in = inList[0:]
		}
		if len(outList) > 1 {
			temp_out = outList[0 : len(outList)-1]
		}
		if len(nOut) > 0 {
			temp_nOut = nOut[0:]
		}
		temp_nOut = append(temp_nOut, outList[len(outList)-1])
		fmt.Println(index, "outList ---> ", inList, outList, nOut)
		TrainRun(temp_in, temp_out, temp_nOut, index+1)
	} 
	fmt.Println(index, " ++++++> ", inList, outList, nOut)
	if len(inList) > 0 { //如果还有未入站的，就进站
		var temp_in, temp_out, temp_nOut []int
		if len(inList) > 1 {
			temp_in = inList[1:]
		}
		if len(outList) > 0 {
			temp_out = outList[0:]
		}
		if len(nOut) > 0 {
			temp_nOut = nOut[0:]
		}
		temp_out = append(temp_out, inList[0])

		TrainRun(temp_in, temp_out, temp_nOut, index+1)

	}
}

func main() {
		
	//火车进站顺序
	var nList []int

	//火车数量
	var nCount int

	var nTemp int
	fmt.Scan(&nCount)
	for i := 0; i < nCount; i++ {
		fmt.Scan(&nTemp)
		nList = append(nList, nTemp)
	}

	inList := nList[0:]
	var outList []int
	var nOut []int

	TrainRun(inList, outList, nOut, 0)
	fmt.Println(nOutList)

}

```











### 考题三 扑克牌找顺子

> 在斗地主扑克牌游戏中， 扑克牌由小到大的顺序为：3,4,5,6,7,8,9,10,J,Q,K,A,2， 
>
>  玩家可以出的扑克牌阵型有：单张、对子、顺子、飞机、炸弹等。 
>
>  其中顺子的出牌规则为：由 **至少 5 张由小到大连续递增** 的扑克牌组成，且 **不能包含 2** 。 
>
>  例如：{3,4,5,6,7}、{3,4,5,6,7,8,9,10,J,Q,K,A}都是有效的顺子； 
>
>  而{J,Q,K,A,2}、 {2,3,4,5,6}、{3,4,5,6}、{3,4,5,6,8}等都不是顺子。 
>
>  给定一个包含13张牌的数组，如果有满足出牌规则的顺子，请输出顺子。 
>
>  如果存在多个顺子，请每行输出一个顺子，且需要按顺子的 **第一张牌的大小（必须从小到大）** 依次输出。 
>
>  如果没有满足出牌规则的顺子，请 **输出 No** 。

```go
//通过率为95%
package main
import (
    "fmt"
    "strings"
    "bufio"
    "os"
    "sort"
)

//查找扑克，并标记为1，已使用
func FindBk(nBk string ,nBKList []string , vFlag []int) bool {
    for i:=0 ; i<len(nBKList) ; i++ {
        if nBk == nBKList[i] && vFlag[i] == 0 {
            vFlag[i] = 1 
            return true
        }
    }
    return false
}


func main() {
    
    inputReader := bufio.NewReader(os.Stdin)
    nStrTemp,_,_:=inputReader.ReadLine() 
    nStr:=strings.ToUpper(string(nStrTemp))
    //拿到的扑克牌
    nBKList := strings.Split(nStr," ")
    //初始化扑克牌顺序序列
    sList := []string{"3","4","5","6","7","8","9","10","J","Q","K","A" }
    
    
    var rstList []string
    //记录扑克牌是否被用过
    vFlag := make([]int , len(nBKList))
    for i:=0 ; i<len(sList)-4; i++ {
        //如果有 sList[i] 牌，则顺序查找有没有顺子
        for FindBk(sList[i],nBKList,vFlag) {
            nRstTemp := sList[i]
            //从sList[i] 牌往后，顺序看看手里的牌能组成顺子
            for k:=i+1;k<len(sList);k++ {
                if FindBk(sList[k],nBKList,vFlag) {
                    nRstTemp += " "+sList[k]
                    //如果 sList[k] 为顺子的最后一张 A ，且长度超过5，则输出
                    if sList[k] == "A" && k-i >=5 {
                        rstList = append(rstList, nRstTemp)
                    }
                } else {
                    //长度超过5，则输出
                    if k-i >=5 {
                        rstList = append(rstList, nRstTemp)
                        
                    }
                    break
                }
            }
            
        }
        
    }
    
    
    if len(rstList) > 0 {
        sort.Strings(rstList)
        for _,v := range rstList {
            fmt.Println(v)
        }        
    } else {
        fmt.Println("No")
    }
}
```



### END

