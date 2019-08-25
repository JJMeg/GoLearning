# Chapter10-channel
本文路线: [goroutine涉及的问题](#goroutine涉及的问题) -> [channel](#channel) -> [channel的声明及初始化](#channel的声明及初始化) -> [channel中存取数据](#channel中存取数据) -> [channel阻塞goroutine](#channel阻塞goroutine) -> [非缓冲channel](#非缓冲channel) -> [死锁案例](#死锁案例) -> [缓冲channel](#缓冲channel)


### goroutine涉及的问题
goroutine是实现并发的手段，协程并发执行的过程中，还涉及到同步问题，同步就是多个协程之间如何传递消息，在此引入channel

### channel
channel 通道，常用于goroutine之间同步消息，发送和接收消息，可视为一个队列，实质是在goroutine之间共享内存。


### channel的声明及初始化
```Go
//声明
var ch chan 类型
//初始化
ch := make(chan 类型)
```

```Go
var ch chan int
ch := make(chan int)
```

### channel中存取数据
```Go
ch <- x //往channel中存入数据 x
<- ch // 从channel中取出数据
x <- ch //从channel中取出数据并赋值给 x
```

### channel阻塞goroutine
```Go
var channel chan int = make(chan int)

func main()  {
	var messages chan string = make(chan string)
	go func (message string)  {
		messages <- message // channel存消息
    // 若未取出这条消息，这个协程将被阻塞
	}
	fmt.Println(<-messages)//channel取消息，消息被取后，阻塞解除
}
```
- 取数据：无缓冲的信道不存储数据，只负责数据的流通，从信道取数据时，若没有数据在信道中，那么当前线程阻塞
- 存数据：没有其他goroutine拿走数据，数据还在信道中，当前这个线程也阻塞


### 非缓冲channel
channel中只存储一个数据

```Go
// 这个channel中只存储一个数据
var ch chan int = make(chan int)

func foo()  {
	ch <- 0//存消息，如果没有其他goroutine来取走这个数据，那么挂起foo，直到main函数把这个0取走
}

func main()  {
	go foo()
	<- ch//从ch取消息，如果ch中没有数据，就挂起main线程，直到foo函数往ch内放数据
}
```

2. 问题：goroutine已经执行完毕了如何去通知main线程？
```Go
var complete chan int = make(chan int)

func loop()  {
	for i:=0 ; i<10 ;i++{
		fmt.Print("%d",i)
	}
	complete <- 0
}

func main()  {
	go loop()
	<- complete //直到线程跑完，取到消息
}
```

### 死锁案例
```Go
//例子1
func main()  {
	var ch := make(chan,int)
	ch <- 1//死锁了，没有线程取走这个数据
	fmt.Println("wrong")
}

//例子2
var ch1 chan int = make(chan int)
var ch2 chan int = make(chan int)
func say(s string)  {
	fmt.Println(s)
	ch1 <- <- ch2//ch1等待ch2的输出
}
func main()  {
	go say("hello")
	<- ch1
}

//例子3
c, quit := make(chan int), make(chan int)
go func() {
   c <- 1  // c通道的数据没有被其他goroutine读取走，堵塞当前goroutine
   quit <- 0 // quit始终没有办法写入数据
}()
<- quit // quit 等待数据的写

//反例
func main() {
    c := make(chan int)
    go func() {
       c <- 1
    }()
}//main未等待其他goroutine，自己先运行完毕了，没有数据流入c通道，一共执行了一个goroutine，没有阻塞，没有死锁
```

### 缓冲channel
容量未满不会阻塞，无缓冲信道从不存储数据，流入的数据必须流出才行，否则将阻塞当前线程

```Go
var ch chan int = make(chan int, 2) // 写入2个元素都不会阻塞当前goroutine, 存储个数达到2的时候会阻塞
func main() {
    ch := make(chan int, 3)
    ch <- 1
    ch <- 2
    ch <- 3
}//若再流入一个数据，信道ch会阻塞main线程，报死锁
```
