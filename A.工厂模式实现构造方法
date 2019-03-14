#### 工厂模式
- Golang中没有构造函数，struct相当于java中的class，如何生成实例？
- 利用工厂模式可以实现构造函数

```go
package model

type Student struct {//Student是大写，若是student时，不可导出如何做到？工厂模式
	Name string
	Score int64
}



package main

import (
	"factory/model"
	"fmt"
	_ "fmt"
)

func main() {
	var stu = model.Student{
		Name:  "tom",
		Score: 89,
	}

	fmt.Println(stu)
}

//{tom 89}
```

- 工厂模式
```Go
package model

type student struct {//student为小写，私有结构体只能在model包内使用，使用工厂模式解决
	Name string
	Score int64
}

func NewStudent(n string, s int64) *student {
	return &student{//创建一个实例，返回一个指针
		Name:n,
		Score:s,
	}
}

package main

import (
	"factory/model"
	"fmt"
	_ "fmt"
)

func main() {
	//私有时，工厂模式解决
	var stu = model.NewStudent("tom",89)
	fmt.Println(*stu)
}

```

### 结构体内的变量也是小写的情况
```Go
package model

type student struct {//student为小写，私有结构体只能在model包内使用，使用工厂模式解决
	name string
	score int64
}

func NewStudent(n string, s int64) *student {
	return &student{//创建一个实例，返回一个指针
		name:n,
		score:s,
	}
}

func (s *student) GetScore() int64  {//s能访问包内变量，GetScore为公开方法
	return s.score//s是值拷贝还是指针拷贝
}



package main

import (
	"factory/model"
	"fmt"
	_ "fmt"
)

func main() {
	//私有时，工厂模式解决
	var stu = model.NewStudent("tom",89)
	fmt.Println(stu.GetScore())//调用公开方法访问私有变量
}
```
