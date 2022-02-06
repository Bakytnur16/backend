Golang的应用方向:
区块链技术:简称BT  
Blockchain technoglogy,也被称为分布式账本技术，是一-种互联网数据库技术，其特点是去中心化，公开透明，让每个人均可参与数据库记录。  

后端服务器应用:
支撑主站后台流量(排序，推荐，搜索等)，提供负载均衡， cache, 容错，按条件分流，统计运行指标美团云计算/云服务的后台应用
CDN的调度系统，分发系统，监控系统，短域名服务，CDN内部开放平台，运营报表系统以及其他一些小工具等。
Golang的计算能力很强

src 目录：放置项目和库的源文件；  
pkg 目录：放置编译后生成的包/库的归档文件；   
bin 目录：放置编译后生成的可执行文件。  
token
keyword25个: beak func go goto interface不认识的： defer（延迟执行），fallthrough，goto（跳转语句），select，go（并发语法） 

Cookies是由服务器产生的。  浏览器第一次访问服务端时，服务器此时肯定不知道他的身份，所以创建一个独特的身份标识数据，格式为key=value，放入到Set-Cookie字段里，随着响应报文发给浏览器。 服务端收到请求报文后，发现Cookie字段中有值，就能根据此值识别用户的身份然后提供个性化的服务。
Cookie是存储在客户端方，Session是存储在服务端方，客户端只存储SessionId   
Token是在服务端将用户信息经过Base64Url编码过后传给在客户端，每次用户请求的时候都会带上这一段信息，因此服务端拿到此信息进行解密后就知道此用户是谁了，这个方法叫做JWT(Json Web Token)。
Token 使服务端无状态化，不会存储会话信息。  

# Golang
包声明： package main表示一个可独立执行的程序，每个 Go 应用程序都包含一个名为 main 的包  
引入包： import "fmt" 告诉 Go 编译器这个程序需要使用 fmt 包（的函数，或其他元素），fmt 包实现了格式化 IO（输入/输出）的函数  
函数： func main() {}  
语句 & 表达式： 
注释： // or /*  */  

Go 程序可以由多个标记组成，可以是关键字，标识符，常量，字符串，符号。如以下 GO 语句由 6 个标记组成
fmt.Println("Hello, World!")  

## 数据类型：  
**整型：**  int(8,16,32,64)，unit(8,16,32,64)，uintptr（无序号整型），byte  
**字符串类型：**
> byte // uint8 的别名  代表ASCII  
> rune // int32 的别名 代表一个 Unicode 码   
字符串跨行写用``

**浮点类型：** float32、float64  
此两个浮点数之间不应该使用＝或  
进行比较操作，高精度科学计算应该使用 math 标准库  

**复数类型：** complex64、complex128  
**布尔型：**  var b bool = true  
**接口类型（interface）** ： error  
空白标识符： _,  

**指针类型（Pointer):**  用*  
- 一个指针变量指向了一个值的内存地址。  
var var_name *var-type  
var ip *int        /* 指向整型*/  
var fp *float32    /* 指向浮点型 */  
var ptr *int(* 指针变量)
ptr =  &a 将给出变量的实际地址。  获取地址  
访问结构体用 '.'  
- nil 指针也称为空指针。
nil在概念上和其它语言的null、None、nil、NULL一样，都指代零值或空值。
- 一个指针变量通常缩写为 ptr。
if(ptr == nil) 
- 结构体指针
var struct_pointer *Books  
struct_pointer = &Book1  指针变量可以存储结构体变量的地址  
struct_pointer.title  使用结构体指针访问结构体成员，使用 "." 操作符：  
```
type Books struct {
	title   string
	author  string
	subject string
	book_id int
}

func main() {
	var Book1 Books
	var Book2 Books

	Book1.title = "GO 语言"
	Book1.author = "www.runoob.com"
	Book1.subject = "Go 语言教程"
	Book1.book_id = 6495407
```
```
a := [...]int{1, 434, 545, 653, 43, 3, 434} //数组
b := make([]int, 2, 3) // 切片类型，抽象列表
c := map[string]int{"a": 1, "b": 2} //字典类型
fmt.Println(a, b, c)
```
**数组:**   
n emetType  
var identifier []type  
var numbers []int  
var balance[10] int 长度为10的整数类型数组  
var balance = [5]float32{1000.0, 2.0, 3.4, 7.0, 50.0}  
balance := [5]float32{1000.0, 2.0, 3.4, 7.0, 50.0}    
长度不确定，可以用。。。代替  
var balance = [...]float32{1000.0, 2.0, 3.4, 7.0, 50.0}  
var salary float32 = balance[9] 数组元素  
```
package main

import "fmt"

func main() {
   var n [10]int /* n 是一个长度为 10 的数组 */
   var i,j int

   /* 为数组 n 初始化元素 */        
   for i = 0; i < 10; i++ {
      n[i] = i + 100 /* 设置元素为 i + 100 */
   }

   /* 输出每个数组元素的值 */
   for j = 0; j < 10; j++ {
      fmt.Printf("Element[%d] = %d\n", j, n[j] )
   }
}


package main

import "fmt"

func main() {
	var i, j, k int
	balance := [5]float32{100.0, 2.0, 3.4, 7.0, 50.0}

	for i = 0; i < 5; i++ {
		fmt.Printf("balance[%d] = %f\n", i, balance[i])
	}

	balance2 := [...]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
	for j = 0; j < 5; j++ {
		fmt.Printf("balance[%d] = %f \n", j, balance2[j])
	}
	balance3 := [5]float32{1: 2.0, 3: 7.0}
	for k = 0; k < 5; k++ {
		fmt.Printf("balance[%d] = %f\n", k, balance3[k])
	}
}

func main() {
	fmt.Printf("%f\n", math.Pi)
	fmt.Printf("%.2f\n", math.Pi) // 用 Printf 函数打印浮点数时可以使用“%f”来控制保留几位小数
}
```

```
type struct_variable_type struct {
   member definition
   member definition
   ...
   member definition
}

type Books struct {
	title   string
	author  string
	subject string
	book_id int
}

func main() {
	fmt.Println(Books{"Go 语言", "www.runoob.com", "Go 语言教程", 6495407})

	fmt.Println(Books{title: "Go 语言", author: "www.runoob.com", subject: "Go 语言教程", book_id: 6495407})

	fmt.Println(Books{title: "Go 语言", author: "www.runoob.com"})
}

variable_name := structure_variable_type {value1, value2...valuen}
或
variable_name := structure_variable_type { key1: value1, key2: value2..., keyn: valuen}

package main

import "fmt"

type Books struct {
	title   string
	author  string
	subject string
	book_id int
}

func main() {
	var Book1 Books
	var Book2 Books

	Book1.title = "GO 语言"
	Book1.author = "www.runoob.com"
	Book1.subject = "Go 语言教程"
	Book1.book_id = 6495407

	Book2.title = "Python 教程"
	Book2.author = "www.runoob.com"
	Book2.subject = "Go 语言教程"
	Book2.book_id = 6495700

	fmt.Printf("Book 1 title : %s\n", Book1.title)
	fmt.Printf("Book 1 author : %s\n", Book1.author)
	fmt.Printf("Book 1 subject : %s\n", Book1.subject)
	fmt.Printf("Book 1 book_id : %d\n", Book1.book_id)

	/* 打印 Book2 信息 */
	fmt.Printf("Book 2 title : %s\n", Book2.title)
	fmt.Printf("Book 2 author : %s\n", Book2.author)
	fmt.Printf("Book 2 subject : %s\n", Book2.subject)
	fmt.Printf("Book 2 book_id : %d\n", Book2.book_id)
}

```


+ 结构化类型(struct)  
+ Channel 类型:chan  
+ 函数类型  

**切片类型(slice)：**  
Go 语言切片是对数组的抽象。Go 数组的长度不可改变，在特定场景中这样的集合就不太适用，Go 中提供了一种灵活，功能强悍的内置类型切片("动态数组")，与数组相比切片的长度是不固定的，可以追加元素，在追加时可能使切片的容量增大。  
- 通过内置函数 make() 初始化切片s（创建切片），[]int 标识为其元素类型为 int 的切片。    
cap() 可以测量切片最长可以达到多少    
```
a := make([]int,10)等于：[0 0 0 0 0 0 0 0 0 0]
capacity 为可选参数
var slice1 []type = make([]type, len) // slice1 := make([]type, len)
var numbers = make([]int, 3, 5)

package main

import "fmt"

func main() {
   var numbers []int
   printSlice(numbers)

   /* 允许追加空切片 */
   numbers = append(numbers, 0)
   printSlice(numbers)

   /* 向切片添加一个元素 */
   numbers = append(numbers, 1)
   printSlice(numbers)

   /* 同时添加多个元素 */
   numbers = append(numbers, 2,3,4)
   printSlice(numbers)

   /* 创建切片 numbers1 是之前切片的两倍容量*/
   numbers1 := make([]int, len(numbers), (cap(numbers))*2)

   /* 拷贝 numbers 的内容到 numbers1 */
   copy(numbers1,numbers)
   printSlice(numbers1)  
   
str := "hello,world！" //通过字符串字面量初始化 个字符串 str
a : = []byte(str)  //将字符串转换为［] byte 类型切片  
b : = []rune(str)   //将字符串转换为［] rune 类型切片    
}

func printSlice(x []int){
   fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x)
}

len=0 cap=0 slice=[]
len=1 cap=1 slice=[0]
len=2 cap=2 slice=[0 1]
len=5 cap=6 slice=[0 1 2 3 4]
len=5 cap=12 slice=[0 1 2 3 4]
```

**Map 类型:** 类似python的字典    
Map是字典类型， Map 是使用 hash 表来实现的。  
var map_variable map[key_data_type]value_data_type  
map_variable := make(map[key_data_type]value_data_type)  

```
package main

import "fmt"

func main() {
	_, numb, strs := numbers()//只获取函数返回值的后两个
	fmt.Println(numb, strs)
}

//一个可以返回多个值的函数
func numbers() (int, int, string) {
	a, b, c := 1, 2, "str"
	return a, b, c
}
```
```
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
}
```
delete() 函数用于删除集合的元素, 参数为 map 和其对应的 key
```
package main

import "fmt"

func main() {
        /* 创建map */
        countryCapitalMap := map[string]string{"France": "Paris", "Italy": "Rome", "Japan": "Tokyo", "India": "New delhi"}

        fmt.Println("原始地图")

        /* 打印地图 */
        for country := range countryCapitalMap {
                fmt.Println(country, "首都是", countryCapitalMap [ country ])
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

## 变量： 
变量的命名规则遵循骆驼命名法，即首个单词小写，每个新单词的首字母大写，例如：numShips   
```
var age int
var a, b int
var b,c int = 1, 2  
var x string = "Roomb"  
fruit = apples + oranges
var d = true
f := "Runoob" // var f string = "Runoob"  := 左侧的变量不应该是已经被声明过的，否则会导致编译错误,这种不带声明格式的只能在函数体中出现  
全局变量：
var (
a int
b string)  
```

## 常量
常量中的数据类型只可以是布尔型、数字型（整数型、浮点型和复数）和字符串型。  
常量还可以用作枚举  
iota，特殊常量，可以认为是一个可以被编译器修改的常量。  
```
const (
	a = iota
	b
	c
)#等于0，1，2
package main

import "unsafe"

const (
	a = "abc"
	b = len(a)
	c = unsafe.Sizeof(a)
)

func main() {
	println(a, b, c)
}


package main

import "fmt"

func main() {
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
0 1 2 ha ha 100 100 7 8
```
a ++ 自增
a -- 自减
&& and || or ! not
```
package main

import "fmt"

func main() {
	var a = true
	var b = false
	if a || b {
		fmt.Printf("第一行 -条件为 true \n")
	}
	if a && b {
		print("第二行 - 条件为true\n")
	}
}
```

### 条件语句
if ..elif ..else
switch  
select  
循环：for
break,continue,goto(将控制转移到被标记的语句)
```
a := [5]int{1, 2, 3, 4, 5}
for k := range a {
	fmt.Println(k)
}#只打印序列0，1，2，3，4

for k，v := range a {
	fmt.Println(k,v)
}#打印序列和值
0 1
1 2
2 3
3 4
4 5

func main() {
	a := [5]int{1, 2, 3, 4, 5}
	for _, v := range a {
		fmt.Println(v)
	}
}
1
2
3
4
5

a := [5]int{1, 2, 3, 4, 5}
b := len(a)
for i := 0; i < b; i++ {
	print(i)
}
```

## 函数
func max(num1, num2 int) int { //输入的类型是int 返回的类型是int  
func function_name( [parameter list] ) [return_types] {

range  
迭代数组(array)、切片(slice)、通道(channel)或集合(map)  
```
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
}
```
### 递归函数  
```
//阶乘
package main

import "fmt"

func Factorial(n uint64)(result uint64) {
    if (n > 0) {
        result = n * Factorial(n-1)
        return result
    }
    return 1
}

func main() {  
    var i int = 15
    fmt.Printf("%d 的阶乘是 %d\n", i, Factorial(uint64(i)))
}
```
// 斐波那契数列
```
package main

import "fmt"

func fibonacci(n int) int {
	if n < 2 {
		return n
	}
	return fibonacci(n-2) + fibonacci(n-1)

}

func main() {
	var i int
	for i = 0; i < 10; i++ {
		fmt.Printf("%d \t", fibonacci(i))
	}

}
```
#### 数据转换
type_name(expression)  
b = int32(a)  
 mean = float32(sum)/float32(count)  
 ```
 package main

import "fmt"

func main() {
	var sum int = 17
	var count int = 5
	var mean float32

	mean = float32(sum) / float32(count)
	fmt.Printf("mean的值为： %f\n", mean)
}
 ```
 
 ### 接口
 Go 语言提供了另外一种数据类型即接口，它把所有的具有共性的方法定义在一起，任何其他类型只要实现了这些方法就是实现了这个接口。
 ```
 /* 定义接口 */
type interface_name interface {
   method_name1 [return_type]
   method_name2 [return_type]
   method_name3 [return_type]
   ...
   method_namen [return_type]
}

/* 定义结构体 */
type struct_name struct {
   /* variables */
}

/* 实现接口方法 */
func (struct_name_variable struct_name) method_name1() [return_type] {
   /* 方法实现 */
}
...
func (struct_name_variable struct_name) method_namen() [return_type] {
   /* 方法实现*/
}
 ```
 ```
 package main

import (
    "fmt"
)

type Phone interface {
    call()
}

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
 ### 处理错误
 ```
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
 ### goroutine 并发
 goroutine 是轻量级线程，goroutine 的调度是由 Golang 运行时进行管理的。  
 go 函数名( 参数列表 )  
 go f(x, y, z)
 
 ### 通道
 通道（channel）是用来传递数据的一个数据结构。

通道可用于两个 goroutine 之间通过传递一个指定类型的值来同步运行和通讯。操作符 <- 用于指定通道的方向，发送或接收。如果未指定方向，则为双向通道。
```
ch <- v    // 把 v 发送到通道 ch
v := <-ch  // 从 ch 接收数据
           // 并把值赋给 v
```
ch := make(chan int) // 声明一个通道很简单，我们使用chan关键字即可
```
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

        c := make(chan int)
        go sum(s[:len(s)/2], c)
        go sum(s[len(s)/2:], c)
        x, y := <-c, <-c // 从通道 c 中接收

        fmt.Println(x, y, x+y)
}
```
通道可以设置缓冲区，通过 make 的第二个参数指定缓冲区大小：
ch := make(chan int, 100)


web：
gin，echo，beego，iris，buffalo，revel
delve调试器

### 标准库
```
bufio	带缓冲的 I/O 操作
bytes	实现字节操作
container	封装堆、列表和环形列表等容器
crypto	加密算法
database	数据库驱动和接口
debug	各种调试文件格式访问及调试功能
encoding	常见算法如 JSON、XML、Base64 等
flag	命令行解析
fmt	格式化操作
go	Go语言的词法、语法树、类型等。可通过这个包进行代码信息提取和修改
html	HTML 转义及模板系统
image	常见图形格式的访问及生成
io	实现 I/O 原始访问接口及访问封装
math	数学库
net	网络库，支持 Socket、HTTP、邮件、RPC、SMTP 等
os	操作系统平台不依赖平台操作封装
path	兼容各操作系统的路径操作实用函数
plugin	Go 1.7 加入的插件系统。支持将代码编译为插件，按需加载
reflect	语言反射支持。可以动态获得代码中的类型信息，获取和修改变量的值
regexp	正则表达式封装
runtime	运行时接口
sort	排序接口
strings	字符串转换、解析及实用函数
time	时间接口
text	文本模板及 Token 词法器
```
