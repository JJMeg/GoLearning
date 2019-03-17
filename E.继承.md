#### 继承基本语法
- 多个结构体存在相同的方法和属性
- 把共有的字段和方法提取出来，特有字段可以自己保留
- 在结构体中匿名结构体实现继承，特有结构嵌入共有结构体即可，匿名结构的大小写属性或方法都可用

```Go
package main

import (
	"fmt"
	_ "fmt"
)

type Person struct {
	age    int64
	salary float64
}

func (p *Person) GetAge() int64 {
	fmt.Println("get age: ", p.age)
	return p.age
}

func (p *Person) GetSalary() float64 {
	fmt.Println("get age: ", p.age)
	return p.salary
}

func (p *Person) SetAge(age int64) {
	if (age <= 0 || age > 120) {
		fmt.Println("age error")
		return
	}
	p.age = age
	fmt.Println("set age: ", p.age)
}

type Student struct {
	Person//嵌入匿名结构体
	grade int64
}

func (s *Student) test() {
	fmt.Println("testing....")
}


func main() {
	stu := &Student{}

	stu.Person.SetAge(3)
	stu.Person.salary=32
  //stu.salary=32 省略中间匿名结构名也可以
	stu.test()
	stu.Person.GetAge()
}

```

- 就近原则
```Go
package main

import (
	"fmt"
	_ "fmt"
)

type Person struct {
	Name string
	age    int64
	salary float64
}

func (p *Person) GetAge() int64 {
	fmt.Println("get age: ", p.age)
	return p.age
}

func (p *Person) GetSalary() float64 {
	fmt.Println("get age: ", p.age)
	return p.salary
}

func (p *Person) SetAge(age int64) {
	if (age <= 0 || age > 120) {
		fmt.Println("age error")
		return
	}
	p.age = age
	fmt.Println("set age: ", p.age)
}

type Student struct {
	Person //嵌入匿名结构体
	grade int64
	Name  string
}

func (s *Student) test() {
	fmt.Println("testing....")
}

func main() {
	stu := &Student{}

	stu.Person.SetAge(3)
	stu.Person.salary = 32
	stu.test()
	stu.Person.GetAge()

	stu.Name = "hhh"//先找Student里头有Name了
	//同理，若方法匿名结构体有，本结构体也有，优先调本结构体的
	//若匿名结构体有方法，结构体没有，将调用匿名结构体的的方法，里面的变量将使用匿名结构体的变量
}
```


__注意：若结构体嵌入多个匿名结构体，匿名结构体有相同字段和方法，同时结构体本身没有，访问时必须明确结构体名字，否则编译报错__
