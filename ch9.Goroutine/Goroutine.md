# Chapter9-Goroutine
本文路线: [Goroutine](#Goroutine) -> [goroutine的声明](#goroutine的声明) -> [goroutine例子](#goroutine例子)

### Goroutine
Go中实现并发的手段，Goroutine的创建和销毁的代价更小，调度也独立于线程，因此Goroutine可视为轻量级的线程，又称协程

### goroutine的声明
在函数前添加`go`关键字即可
```Go
say(){
  fmt.Println("hello world")
}

func main()  {
  say()
  go say()// 创建了一个新协程
}
```
### goroutine例子
1. 未引入goroutine，程序串行执行

```Go
package main

import "fmt"

func loop(round string)  {
	for i := 1; i<=3 ;i++{
		fmt.Printf("Round:%s,\ti:%d\n",round,i)
	}
}

func main()  {
	loop("A")
	loop("B")
}
```

loop是按顺序执行的，先执行loop(1)，再执行loop(2)

```shell
Round:A,	i:1
Round:A,	i:2
Round:A,	i:3
Round:B,	i:1
Round:B,	i:2
Round:B,	i:3
```

2. 引入goroutine

```Go
package main

import "fmt"

func loop(round string)  {
	for i := 1; i<=3 ;i++{
		fmt.Printf("Round:%s,\ti:%d\n",round,i)
	}
}

func main()  {
	go loop("B")
  loop("A")
}
```
只运行了A轮结果，主线程运行，主线程也是一个goroutine，loop("A")在主线程中
中运行loop("A")，新起一个goroutine运行loop("B")，goroutine没来得及跑loop("B")，主线程已经结束，所以只有loop("A")的结果
```shell
Round:A,	i:1
Round:A,	i:2
Round:A,	i:3
```

3. 等待goroutine结束
```Go
package main

import (
  "fmt"
  "time"
)

func loop(round string)  {
	for i := 1; i<=3 ;i++{
		fmt.Printf("Round:%s,\ti:%d\n",round,i)
	}
}

func main()  {
	go loop("B")
  loop("A")
  go loop("C")
  time.Sleep(time.Second)
}
```
先运行A轮再运行B轮，主线程建立后，新起一个goroutine运行loop("B")，主线程执行A轮，A轮执行完，有一秒钟等待时间，goroutine来得及跑loop("B")，再出现B轮的结果
```shell
Round:A,	i:1
Round:A,	i:2
Round:A,	i:3
Round:B,	i:1
Round:B,	i:2
Round:B,	i:3
```

4. sync.WaitGroup实现等待goroutine结束
```Go
package main

import (
    "fmt"
    "sync"
)

func goRun(round string, wg *sync.WaitGroup)  {
  defer wg.Done()
  loop(round)
}
func loop(round string)  {
	for i := 1; i<=3 ;i++{
		fmt.Printf("Round:%s,\ti:%d\n",round,i)
	}
}

func main()  {
  var wg sync.WaitGroup

  wg.Add(3)
  go goRun("A",&wg)
  go goRun("B",&wg)
  go goRun("C",&wg)

  wg.Wait()
}
```

使用 `sync.WaitGroup` 来完成等待的操作，比起`time.Sleep()`更灵活，这种 等待是协程之间实现同步的办法

```shell
Round:C,	i:1
Round:C,	i:2
Round:C,	i:3
Round:A,	i:1
Round:A,	i:2
Round:A,	i:3
Round:B,	i:1
Round:B,	i:2
Round:B,	i:3
```
