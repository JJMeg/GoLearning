#### Golang抽象
```Go
package main

import (
	"fmt"
	_ "fmt"
)

type Account struct {
	AccountNum string
	Pwd string
	Balance float64
}

//方法
func (account *Account) SaveMoney (money float64, pwd string){
	if (pwd != account.Pwd){
		fmt.Println("Password error!")
		return
	}else if(money <= 0 ){
		fmt.Println("money error")
		return
	}

	account.Balance += money
	fmt.Println("Save money success")
}

func (account *Account) GetMoney (money float64, pwd string){
	if (pwd != account.Pwd){
		fmt.Println("Password error!")
		return
	}else if(money > account.Balance || money == 0 ){
		fmt.Println("money error")
		return
	}

	account.Balance -= money
	fmt.Println("get money success")
}

func (account *Account) Query (pwd string)  {
	if (pwd != account.Pwd){
		fmt.Println("password error")
		return
	}

	fmt.Println("query result is ~~~~")
}
func main() {
	var acc = &Account{
		Pwd:"123",
		AccountNum:"qaz",
		Balance:123.0,
	}
	acc.GetMoney(12,"123")
	acc.Query("qwee")
	acc.SaveMoney(100.0,"123")
}

//get money success
//password error
//Save money success
```

 
