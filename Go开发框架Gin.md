#### Gin介绍
- 用Go编写的Web框架，最快的路由httprouter
- 只需引入包、定义路由、编写handler
- 官方文档：https://github.com/gin-gonic/gin
- 其他搬运工：https://www.yoytang.com/go-gin-doc.html

#### 常规用法
- RESTful API
    ```Go
      package main

      import (
        "github.com/gin-gonic/gin"
        "net/http"
      )

      func main(){
        engine := gin.Default()//初始化引擎
        engine.Any("/",WebRoot)//注冊一个路由和处理函数
        engine.Run(":4040")//绑定一个端口然后启动应用，默认绑定为8080
        engine.GET("/getID",getID)
        engine.PUT("/updateID",updateID)
        engine.DELETE("/deleteByID",deleteByID)
        //GET PUT POST DELETE PATCH HEAD OPTIONS
      }

      func WebRoot(context *gin.Context){
        context.String(http.StatusOK, "hello world")
      }

      func getID(){

      }

      func updateID()  {

      }

      func deleteByID()  {

      }
    ```

  - 带参数的API，即动态路由

    ```Go
    package main

    import (
      "github.com/gin-gonic/gin"
      "net/http"
    )

    func main(){
      engine := gin.Default()//初始化引擎

      engine.GET("/user/:id",getByID)
      engine.PUT("/updateID",updateID)
      engine.DELETE("/deleteByID",deleteByID)
      //GET PUT POST DELETE PATCH HEAD OPTIONS

    }

    func getByID(context *gin.Context){
      //使用context.Param(key)可以获取url参数，返回的格式都是string类型
      id := context.Param("id")
      //或 id:= context.Query("id")
      context.String(http.StatusOK, "the id is ", id)
    }
    ```

  - API组

    ```Go
    package main

    import (
      "github.com/gin-gonic/gin"
      "net/http"
    )

    func main(){
      engine := gin.Default()//初始化引擎

      v1 := engine.Group("/v1"){
        v1.GET("/getID",getID)
        v1.PUT("/updateID",updateID)
        v1.DELETE("/deleteByID",deleteByID)
        //GET PUT POST DELETE PATCH HEAD OPTIONS
      }

      v2 := engine.Group("v2")
      v2.GET("/getID",getID2)
      v2.PUT("/updateID",updateID2)
      v2.DELETE("/deleteByID",deleteByID2)
    }

    func getID(context *gin.Context){
      context.String(http.StatusOK, "hello world")
    }
    ```

  - Middleware中间件
    - 可以指定路由要执行的中间环节，例如身份验证，集中处理返回的数据等
    - 单个api用中间件

      ```Go
        package main

        import (
          "github.com/gin-gonic/gin"
          "net/http"
        )

        func main(){
          engine := gin.Default()//初始化引擎
          //有中间件middleware1,middleware2
          v1 := engine.Group("/v1"){
            v1.GET("/getID",middleware1,middleware2,handler)
            v1.PUT("/updateID",updateID)
            v1.DELETE("/deleteByID",deleteByID)
            //GET PUT POST DELETE PATCH HEAD OPTIONS
          }
        }

        func handler(context *gin.Context){
          context.String(http.StatusOK, "hello world")
        }

        func middleware1(context *gin.Context){
          log.Println("exec middleware1")
          //处理代码

          context.Next()
        }

        func middleware2(context *gin.Context)  {
          log.Println("welcome middleware2")
          context.Next()//执行到这里时，gin直接跳到流程的下一个方法中，等那个执行完才回来继续执行middleware2剩下的部分
          //  v1.GET("/getID",middleware1,middleware2,handler)顺序是middleware1,middleware2,handler
          //  middleware2调用了Next，则先执行handler，handler执行完了，才跳回middleware2执行Next之后的代码
        }
        //上述代码执行结果：
        // exec middleware1
        // welcome at middleware2
        // exec handler
        // exec middleware2
      ```

    - api组使用中间件
      ```Go
        package main

        import (
          "github.com/gin-gonic/gin"
          "net/http"
        )

        func main(){
          engine := gin.Default()//初始化引擎
          //有中间件middleware1,middleware2
          v1 := engine.Group("/v1",middleware1){
            v1.GET("/getID",getID)
            v1.PUT("/updateID",updateID)
            v1.DELETE("/deleteByID",deleteByID)
          }
        }

        func handler(context *gin.Context){
          context.String(http.StatusOK, "hello world")
        }

        func middleware1(context *gin.Context){
          log.Println("exec middleware1")
          //处理代码

          context.Next()
        }

        func middleware2(context *gin.Context)  {
          log.Println("welcome middleware2")
        }
        ```

    - 数据绑定

      ```Go
        //url上的参数和struct绑定
        package main

        import (
          "github.com/gin-gonic/gin"
          "net/http"
        )

        type Person struct{
          Name string `json:"name"`
          Age string `json:"age"`
          Address string `json:"address"`
        }

        func main(){
          engine := gin.Default()//初始化引擎
          v1.GET("/test",getInfo)
        }

        func getInfo(context *gin.Context)  {
          var p Person
          if context.ShouldBindQuery(&p) == nil {
            //绑定url查询参数和POST的数据，会根据content-type的类型，优先匹配JSON或者XML
            log.Println("..")
          }
        }
      ```
