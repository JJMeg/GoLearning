#### 封装
- 把抽象出的字段和方法封装到一起，数据保护在内，其他包内只能通过被授权的方法去操作字段，隐藏了实现的细节，运用工厂模式

```Go
package model

import "fmt"

type Person struct {
	age    int64
	salary float64
}

func NewPerson(age int64) *Person  {
	return &Person{
		age:age,
	}
}
func (p *Person) SetAge(age int64) {
	if (age <= 0 || age > 120) {
		fmt.Println("age error")
		return
	}
	p.age = age
	fmt.Println("set age: ",p.age)
}

func (p *Person) GetAge() int64 {
	fmt.Println("get age: ",p.age)
	return p.age
}


package main

import (
	"factory/model"
	_ "fmt"
)

func main() {
	var p = model.NewPerson(10)
	p.SetAge(10)
	p.GetAge()
}

```

