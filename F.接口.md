1. 接口不能创建实例，但可以指向一个自定义类型的变量，例如结构体可以实现接口
2. 接口中所有的方法没有方法体，都没有实现细节
3. 实现某个接口中的所有方法才能称得上是接口实现
4. 只要是自定义类型都可以实现接口，不一定非得结构体
```Go
type AInterface interface{
  Say()
}
type integer int

func (i integer) Say(){
  fmt.Println("hh")
}

func main(){
  var i integer = 10
  var b AInterface = i
  b.Say()
}
```

5. 一个自定义类型可以实现多个接口
6. 接口中不可以有任何变量
```Go
type AInterface interface{
  Name string //不可以
  Say()
}
```

7. 接口可以嵌套接口，但实现接口时必须全部实现
