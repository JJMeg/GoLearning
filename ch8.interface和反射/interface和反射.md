# Chapter8-interface和反射
本文路线: [interface](#interface) -> [interface的声明](#interface的声明) -> [interface的实现](#interface的实现) -> [interface嵌套](#interface嵌套) -> [反射](#反射) ->

### interface
interface 接口，即一些方法的集合，在Go中，interface也是一种类型，属于指针类型，常用接口实现多态

### interface的声明
```Go
type 接口名 interface{
  方法1
  方法2
  ...
}
```

```Go
type AInterface interface{
  Say()
}
```

### interface的实现
1. 接口中所有的方法没有方法体，都没有实现细节，也不能包含变量
```Go
type AInterface interface{
  Say()
  Name string //这里会报错，不可以包含变量
}
```

2. 接口不能创建实例，但可以指向一个自定义类型的变量，只要是自定义类型均可以实现接口，例如结构体可以实现接口
- 错误示例：非自定义类型实现自定义接口
int类型是Go的内置类型，我们自定义的AInterface接口在Go中未实现，因此自定义接口需要由自定义的类型实现
  ```Go
  package main

  import "fmt"

  type AInterface interface{
    Say()
  }

  func (i int) Say(){// 这里报错：cannot define new methods on non-local type int
    fmt.Println("hello")
  }

  func main(){
    var i int = 10
    var b AInterface = i // cannot use i (type int) as type AInterface in assignment:
  	                     // int does not implement AInterface (missing Say method)
    b.Say()
  }
  ```

- 正确示例：自定义类型实现自定义接口

  ```Go
  package main

  import "fmt"

  type AInterface interface{
    Say()
  }

  // 自定义了类型integer
  type integer int

  // 用integer去实现AInterface接口
  func (i integer) Say(){
    fmt.Println("hello")
  }

  func main(){
    var i integer = 10
    var b AInterface = i
    b.Say()
  }
  ```
  ```
  hello
  ```

3. 实现某个接口中的所有方法才能称得上是接口实现
```Go
package main

import "fmt"

type AInterface interface{
  Say()
  Write()
}

type integer int

// 用integer去只实现了接口中的Say方法
func (i integer) Say(){
  fmt.Println("hello")
}

func main(){
  var i integer = 10
  var b AInterface = i
  b.Say()
}
```

由于只实现了部分方法，编译将报错，提示未实现Write方法
```shell
cannot use i (type integer) as type AInterface in assignment:
	integer does not implement AInterface (missing Write method)
```

5. 一个自定义类型可以实现多个接口
```Go
package main

import "fmt"

type AInterface interface{
  Say()
}

type BInterface interface{
  Write()
}

type integer int

// 实现AInterface接口
func (i integer) Say(){
  fmt.Println("hello")
}

// 同时实现BInterface接口B
func (i integer) Write(){
   fmt.Println("world")
}

func main(){
  var i integer = 10
  var b AInterface = i
  b.Say()

  var c BInterface = i
  c.Write()
}
```

```shell
hello
world
```

### interface嵌套
接口可以嵌套接口，但实现接口时必须全部实现

```Go
package main

import "fmt"

type AInterface interface{
  Say()
  BInterface//嵌套B接口
}

type BInterface interface{
  Write()
}

type integer int

func (i integer) Say(){
  fmt.Println("hello")
}

func (i integer) Write(){
   fmt.Println("world")
}

func main(){
  var i integer = 10
  var b AInterface = i
  b.Say()

  var c BInterface = i
  c.Write()
}
```

---
### 反射
