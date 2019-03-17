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
	stu.test()
	stu.Person.GetAge()
}

```
