## 基础

### 类型定义

函数

```go
func sayHello(s string) string {
	return "Hello " + s
}
```

浮点型，推荐使用float64 

``var f float32 = 0.111``

数组

`var beatles [4]string`

```go
    //字符数组定义
    var str [2]string
    str[0] = "world"
    str[1] = "hello"
    fmt.Println(str)
    //赋值数组的第二种方法（应该叫做切片）
    str1 := [2]string{"12", "safas"}
    fmt.Println(str1) 
```

整数型

```go
var s int64 =10
q:=11   //这种定义只能用在函数内部
fmt.Println (reflect.TypeOf (s))
fmt.Println (reflect.TypeOf (q))
```

常量

```go
const greeting string = ” Hello,world”
//常量一般写在外面，就不能使用const greeting:=” Hello,world”
```



### 转换类型

使用strconv进行转换

```
ParseBool：转换为布尔型
FormatBool：布尔型转换为string
```

int和float一样，字符型稍微不同给，使用Itoa和Atoi

```
ad:="131"
var o,_=strconv.Atoi(ad)
```



### 指针

使用&获取变量地址,使用*从地址获取值

```go
ad:="131"
fmt.Print(&ad)
fmt.Print(*&ad)
```





## 函数

### 不定参函数

```go
func sumNumbers(numbers...int) int {
   total:=0
   for _,n:=range numbers{
      total+=n
   }
   return total
}
```

使用...int 表示输入的所有int型参数，遍历后面再说。



### 使用具名返回值

```go
func sayHi () (x,y string) {
   x = "hello"
   y = "world"
   return
}
```

不推荐这么写



### 函数作为参数

```go
func anotherFunction(f func() string ) string {
   return f()
}
```

第一个string表示f函数的返回类型，第二个才表示anotherFunction的返回类型



## 控制流程

### switch语句

```
switch i {
case 1:
   fmt.Print("1")
case 2:
   fmt.Print("1")
default:
   fmt.Print("3")
}
```

### for语句

```
for i:=0;i<10;i++{
   ...
}
```

包含range

```go
for i,n:=range numbers{
    ...
}
```

i序号，n值

### defer

让函数返回前执行另一个函数

```
defer fmt.Println ("I am run after the function completes")
fmt.Println ( "Hello World 1")
```

需要注意的是，如果有多条defer，那么这些瑜伽裤执行顺序是从后往前



## 数组、切片和映射

### 创建方式

数组 var cheeses [2]string
切片 var cheeses = make ( []string , 2)

​	推荐方式  a:=[]int{1,2}

映射 var players= make(map[string ］int)



### 切片操作

增加  cheeses : = append(cheeses ,”Camembert ” )

删除  cheeses= append(cheeses[ : 2] , cheeses[2+1 : ] .. . )

​	其实就是截取0到2的元素（不包括2）和3到最后的元素进行拼接

​	==注意要在后面加上3个点==，append内置方法的定义

​	func append(slice []Type, elems ...Type) []Type　

复制  copy (smellyCheeses , cheeses)

​	复制值，互相不会影响



### 映射操作

就是和字典一样

删除元素  delete{player s,”cook”)



## 结构体和指针

### 机构体的创建

```go
type Movie struct {
	Name string
	Rating float32
}
m :=Movie{
	Name ："simon",
	Rating : 10 ,
}
m= new(Movie)
```

l两种创建方式，推荐上面的

==注意==

1. 创建时不要加逗号，实现时都要加，最后也有
2. 输出的时候要使用fmt.Printf("%+v",m)，否则只有key没有值

### 结构体操作

比较  a=b

​	只是比较数值是否相同，不像是java比较类

赋值  a:=b

​	复制了一个bgei啊，两者互相不影响

赋予指针  a:=&b

​	两者相同，互相影响



## 方法和接口

### 方法的创建

```go
func (m *Movie) summary () string {
	//code
}
func  summary (m *Movie) string {
	//code
}
```

上面是结构体的方法，下面是函数，功能相同，但是调用方式不同



### 接口的创建

```go
type Robot interface {
	PowerOn() err
}
```

实现

```go
type T850 struct {
	Name string
}
func (a *T850) PowerOn() err {
	return nil
}
```

使用接口作为参数

```go
func Boot (r Robot) error {
	return r.PowerOn(}
}
```

没看懂接口是怎么用的，很奇怪的调用方式



## 字符串

### 创建字符

可以使用“”，\t\n等会转义

也可以使用``，输出原始字符

另外字符也可以当作字节的数组来访问，类似python



### 拼接字符

使用var buffer bytes.Buffer，类似java的stringbuilder，具体使用方式为：

```go
var buffer bytes.Buffer
for i := 0 ; i < 50 0 ；工＋＋｛
	buffer. WriteString (” Z " )
}
fmt.Println(buffer.String())
```



### 处理字符

输出第一个字节

​	fmt.Printf("%q",s[0])

​	直接输出是ascii码，另外中文是3个字节

转换为小写

​	strings . ToLower (” VERY IMPORTANT MESSAGE ” ))

查找

​	strings.Index (” surface ” , ” face ” )

​	返回3，找不到返回-1



## 处理错误

### 错误处理

```
file,err:=ioutil.ReadFile("")
if err != nil {
    fmt.Println(err)
    return
}
fmt.Println(file)
```

使用nil判断是否出错，goland快捷键为err.nn就可以输出



### 函数返回错误

```
func Half(num int)(int,error){
	if(num%2==1){
		return -1,fmt.Errorf("error")
	}
	return num/2, nil
}
```



### panic的使用

panic 是Go 语言中的一个内置函数，它终止正常的控制流程并引发恐慌（ panicking ) ，导致程序停止执行。

出现普通错误时，并不提倡这种做法，因为程序将停止执行，并且没有任何回旋余地。

```
if err != nil {
   panic("fatal error")
}
```



## Goroutine和通道

### 使用go处理并发操作

```
func slow()  {
   time.Sleep(time.Second)
   fmt.Println("slow finish")
}
func main() {
   go slow()
   fmt.Println("main")
   time.Sleep(time.Second*2)
}
```

如果main没有sleep，就会直接结束，只有输出一个main



### 使用通道

```
func slow(c chan string)  {
   time.Sleep(time.Second)
   fmt.Println("slow finish")
   c <- "s"
}
func main() {
   c:= make(chan string)
   go slow(c)
   msg:=<-c
   fmt.Println(msg)
}
```

声明变量msg，用于接收来自通道c 的消息。这将阻塞进程直到收到消息为止，从而避免进程过早退出。



### 使用缓冲通道

```
import (
    "fmt"
    "math/rand"
    "sync"
    "time"
)

const (
    numberGoroutines = 4
    taskLoad         = 10
)

var bufferWg sync.WaitGroup

func init() {
    rand.Seed(time.Now().Unix())
}

func main() {
    //创建了一个10任务的缓冲通道
    tasks := make(chan string, taskLoad)
    bufferWg.Add(numberGoroutines)

    //创建4个Goroutine
    for gr := 1; gr <= numberGoroutines; gr++ {
        go worker(tasks, gr)
    }

    //向缓冲通道中放入数据
    for post := 1; post <= taskLoad; post++ {
        tasks <- fmt.Sprintf("Task : %d", post)
    }

    close(tasks)

    bufferWg.Wait()
}

func worker(tasks chan string, worker int) {
    defer bufferWg.Done()

    for {
        task, ok := <-tasks
        if !ok {
            fmt.Printf("Worker: %d : 结束工作 \n", worker)
            return
        }

        fmt.Printf("Worker: %d : 开始工作 %s\n", worker, task)

        //随机处理一下工作的时间
        sleep := rand.Int63n(100)
        time.Sleep(time.Duration(sleep) * time.Millisecond)

        fmt.Printf("Worker: %d : 完成工作 %s\n", worker, task)
    }
}
```

书中的例子有点问题，网上找了一个

`WaitGroup` 对象内部有一个计数器，最初从0开始，它有三个方法：`Add(), Done(), Wait()` 用来控制计数器的数量。`Add(n)` 把计数器设置为`n` ，`Done()` 每次把计数器`-1` ，`wait()` 会阻塞代码的运行，直到计数器地值减为`0`。

参考：

[缓冲通道](https://www.cnblogs.com/dcz2015/p/10384067.html)

[WaitGroup的用法](https://blog.csdn.net/u013474436/article/details/88749749)



### 通道用作函数参数

＜－位于关键字ch an 左边时，表示通道在函数内是只读的； ＜－位于关键宇chan 右边时，表示通道在函数内是只写的；没有指定＜－时， 表示通道是可读写的。

```
func slow(c chan<- string)  {
   pass
}
func slow(c <-chan string)  {
   pass
}
```



### 使用select语句

![](https://gitee.com/wowoa/typoraPic/raw/master/image/20200504211538.png)

select 语句建了两个接收者，分别用于接收来自通道channel1和channel2 的消息。

Is 后，函数pin g ！返回，并向通道c hannell 发送一条消息。

收到来自通道channel ！的消息后，执行第一条case 语句一向终端打印一条消息。

整个select 语句就此结束，不再阻塞进程，因此程序退出。

还可以添加超时时间

```
case <-time.After(500*time.Millisecond):
   fmt.Println("error")
```

感觉不是很实用



## 使用包

安装包

go get 路径



创建包

导入这个包后，就可使用其中所有以大写字母打头的标识符了。要创建私有标识符（变量、函数等），可让它们以小写字母打头。

注意：同一个文件夹里的包名字都是相同的



## 命令行程序

```
for i,arg:=range os.Args{
   fmt.Println(i,arg)
}
```

读取所有的输入



### 命令行标志

```
s:=flag.String("s","hello","test")
flag.Parse()
fmt.Println(s)
```

flag.String 能够让您声明命令行标志，并指定其名称、默认值和帮助文本。

直接运行就会输出hello 

也可以在后面添加参数 -s me,那么就会输出me



### 帮助文本

也就是--help会输出的内容

```
flag.Usage= func() {
      usageText:=`
an example
-s,--s     example string `
   fmt.Fprint(os.Stderr,"%s\n",usageText)
   }
```



### 创建子命令

如git commit和git clone

```
switch os.Args[1]{
case "clone":
   //
case "delete":
   //
default:
   //
}
```



### 完整的命令行程序

![](https://gitee.com/wowoa/typoraPic/raw/master/image/20200505140852.png)



## 网络编程

### 基础的请求

![](https://gitee.com/wowoa/typoraPic/raw/master/image/20200507223127.png)





### 添加head的请求

```
func main() {
	url:=""
	postData:=strings.NewReader(`{"param":""}`)
	req,_:=http.NewRequest("POST",url,postData)
	req.Header.Add("Cookie","")
	response,err:=http.DefaultClient.Do(req)
	if err!=nil {
		log.Printf(err.Error())
	}
	defer response.Body.Close()
	body,err:=ioutil.ReadAll(response.Body)
	if err!=nil {
		log.Printf(err.Error())
	}
	fmt.Printf(string(body))
}
```



### 处理超时

即使用自定义的client，而不是默认的

```
client=&http.Client{
   Timeout:1*time.Second,
}
```



## 处理JSON

### 结构体转换为json

![](https://gitee.com/wowoa/typoraPic/raw/master/image/20200507224555.png)

解决结构体种字段名字如Name应该改为name，并且当字段没有值就忽略，应该设置为

![](https://gitee.com/wowoa/typoraPic/raw/master/image/20200507224845.png)

### json转换

可以转换为结构体，但是最方便的还是转换为map

```
func main() {
   m:=make(map[string]interface{})
   s:=`{"param":"1","type":"money"}`
   json.Unmarshal([]byte(s),&m)
   fmt.Printf("%+v",m)
}
```

同理上面post获取的数据也可以这样转换为map



## 处理文件

### 操作文件

读取

![](https://gitee.com/wowoa/typoraPic/raw/master/image/20200507225344.png)

注意如果是绝对路径，一定要改为/，不能是\。会找不到文件



创建

![](https://gitee.com/wowoa/typoraPic/raw/master/image/20200507225539.png)

注意权限的设置

![](https://gitee.com/wowoa/typoraPic/raw/master/image/20200507225450.png)



复制

```
func main() {
   from,_:=os.Open("")
   to,_:=os.OpenFile("",os.O_RDWR|os.O_CREATE,0666)
   io.Copy(to,from)
}
```

注意一个是open一个是openfile

使用函数OpenFil e 打开文件。第一个参数是要打开（如果不存在， 就创建）的文件的名称； 第二个参数是用于文件的标志，在这里指定的是读写文件， 并在文件不存在时创建它； 最后一个参数设置文件的权限。



删除

os.remove





