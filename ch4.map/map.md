# Chapter4-map
本文路线: [What is map?](#What+is+map?) -> [map的初始化](#map的初始化) -> [map插入元素操作](#map插入元素操作) -> [map删除元素操作](#map删除元素操作) -> [map访问及遍历操作](#map访问及遍历操作)

### What is map?
**键值对集合**

### map的初始化
`make(map[T]T)`

```Go
// 创建一个key为string，value为int类型的map
map1 := make(map[string]int)
// 复合map，key为string，value为一个map
map2 := make(map[string]map[string]int)
```

### map插入元素操作
```Go

package main

import	"fmt"

func main() {
  // 声明map
	var personAge map[string]int
  // 初始化map
	personAge = make(map[string]int)

  // 插入数据
	personAge["Amy"] = 1
	personAge["Daming"] = 2

	fmt.Println("Create a map": personAge)
}
```
```shell
Create a map : map[Daming:2 Amy:1]
```

### map删除元素操作

使用内置函数`delete(map,key)`
```Go
package main

import "fmt"

func main() {
  // 声明map
  var personAge map[string]int
  // 初始化map
  personAge = make(map[string]int)

  // 插入数据
	personAge["Amy"] = 1
	personAge["Daming"] = 2

	fmt.Println("Create a map :",personAge)

  // 删除一个数据
	delete(personAge, "Amy")
	fmt.Println("delete Amy :",personAge)
}

```
```shell
Create a map : map[Daming:2 amy:1]
Delete Amy : map[Daming:2]
```

### map访问及遍历操作
- 访问元素
```Go
package main

import "fmt"

func main() {
  personAge := map[string]int{
  	"Amy" : 1,
  	"Daming" : 2,
  	"Lily" : 3,
  }

  // 1. 通过key的内容直接获取：不存在则返回值类型的0值，例如map[string]string，不存在的返回""
	fmt.Println("Get Jimmy :",personAge["Jimmy"])

  fmt.Println("Get Amy :",personAge["Amy"])

  // 2. personAge["Jimmy"]返回两个参数
  value ,ok := personAge["Jimmy"]
  fmt.Println("Jimmy exists ? ",ok)

  value ,ok = personAge["Amy"]
  fmt.Printf("Amy exists ? %v the value is : %d", ok, value)
}

```
**在访问元素时，第一个参数是访问的元素的值，第二个参数为元素是否存在，存在则是true，不存在为false，此外，不存在的元素的返回值是元素取值的默认0值。**

```shell
Get Jimmy : 0
Get Amy : 1
Jimmy exists ?  false
Amy exists ? true the value is :1
```

- 遍历元素
```Go
package main

import "fmt"

func main() {
  personAge := map[string]int{
  	"Amy" : 1,
  	"Daming" : 2,
  	"Lily" : 3,
  }

  for key, value := range personAge {
    fmt.Printf("key: %s, value: %d\n", key, value)
  }
}
```

```shell
key: Amy, value: 1
key: Daming, value: 2
key: Lily, value: 3
```
