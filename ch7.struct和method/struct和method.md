# Chapter7-struct和method
本文路线: [struct结构体](#struct结构体) -> [struct的声明](#struct的声明) -> [struct的初始化](#struct的初始化) -> [struct的嵌套](#struct的嵌套) -> [成员的可见性](#成员的可见性) -> [method](#method) -> [method的声明](#method的声明) -> [一些约束](#一些约束)

### struct结构体

Go没有类的概念，将多个不同类型的数据集合在一起，可以用到struct


### struct的声明
```Go
type 结构名 struct{
  变量名1 变量类型
  变量名2 变量类型
  ...
}
```

例如

```Go
type Person struct {
	Name     string
	Age      int
	Region   string
}
// 也可以简写为
type Person struct {
	Name, Region   string
	Age              int
}
```

### struct的初始化
```Go
package main

import "fmt"

type Person struct {
	Name     string
	Age      int
	Region   string
}

func main() {
  // 声明
  // 1. 指定变量类型
	var amy Person
	fmt.Println(amy)

  // 2. 构造空结构体，也可以完成声明
  lily := Person{}
  fmt.Println(lily)

  // 结构体变量初始化
  // 1. 对相应字段赋值
	amy.Name = "amy"
	amy.Age = 10
	amy.Region = "China"
	fmt.Println(amy)

  // 2. 直接写在结构体中
  lily = Person{
    Name: "lily",
    Age: 10,
    Region: "China",
  }
  fmt.Println(lily)

  // 修改结构体中的变量
	var p = &amy
	p.Region = "USA"
	fmt.Println(amy)
}

```

```shell
{ 0 }
{ 0 }
{amy 10 China}
{lily 10 China}
{amy 10 USA}
```

### struct的嵌套

1. 嵌入本身的结构体，使用指针嵌入

```Go
type Tree struct {
	Value     int
	Left      *Tree
	Right     *Tree
}
```

2. 嵌入其他结构体

变相实现了继承，Person拥有了Region的所有属性

```Go
type Person struct {
  Name      string
  Age       int
  Location  Region
}

type Region struct {
  Country string
  City    string
}
```

3. 嵌入匿名成员

匿名成员即没有变量命名的成员，可以直接访问匿名成员结构体内的变量
```Go
package main

import "fmt"

type Person struct {
  Name      string
  Age       int
  Region
}

type Region struct {
  Country string
  City    string
}

func main() {

  region := Region{
    Country:  "China",
    City:     "Beijing",
  }

  lily := Person{
    Name: "lily",
    Age: 10,
    Region: region,
  }

  // 可以直接访问Region中的Country和City
  fmt.Println("Her country is: ", lily.Country)
}

```

```shell
Her country is:  China
```

### 成员的可见性

成员的可见性是对外部package而言的，若某个变量对外部包可见，那么称这个变量是可导出的，在Go中，通过成员首字母大小写表明可见性，大写为可导出，小写不可导出，不可导出的成员只能在同一个包内访问

```Go
package person

type Person struct {
	Name     string
	Age      int
	loaction string
}
// Person结构体对外可见，但是其内部的变量只有Name和Age成员对外可见

```

```Go
package main

import "fmt"

func main() {

  lily := Person{
    Name: "lily",
    Age: 10,
    location: "China",//这里会报错，外部的main包无法方位person包中Person结构体的不可见成员
  }
}

```
---

### method
method 方法，附属给一个接收者的函数，接收者是某种类型的变量

### method的声明
```Go
func (接收者变量名称 接收者变量类型) 方法名() 返回值 {
  ...
}
```

```Go
func (person *Person) Print(name string) bool {
  fmt.Println(name)
  return true
}
```

### 一些约束
- 只能为当前所在包内的类型定义方法
- 接收者可为值类型和指针类型，不可为接口类型和指针的指针类型
- 接收者为指针类型时，可用于修改其内容，接收者为值类型时不会改动其内容

```Go
// 接收者为指针类型
func (person *Person) Setname(name string) bool {
  fmt.Println(person.Name)

  person.Setname(name)

  fmt.Println(person.Name)
  return true
}
// 接收者为值类型
func (person Person) Print() bool {
  fmt.Println(&person)
  return true
}
```

- 同一个接收者的方法不可重名，若接收者是一个结构体，其方法名不可以和该结构体内的成员重名
