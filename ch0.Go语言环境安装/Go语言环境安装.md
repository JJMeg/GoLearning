# Chapter0-Go语言环境安装

本文路线: [安装包](#安装包) -> [安装](#安装) -> [配置环境变量](#配置环境变量) -> [检测是否安装成功](#检测是否安装成功) -> [开发工具](#开发工具) -> [Hello World!](#Hello+World!) -> [Go多版本管理](#Go多版本管理) -> [卸载Go](#卸载Go)

### 安装包
https://golang.org/dl/

|OS|
|-|
|FreeBSD 10.3以上|
|Linux 2.6.23以上|
|macOS 10.10以上|
|Windows 7以上|

### 安装
1. Linux/FreeBSD
> $ tar -C /usr/local -xzf go1.2.1.linux-amd64.tar.gz

2. macOS
    - `$ brew install go` 默认安装最新版本
    - `$ tar -C /usr/local -xzf go1.2.1.linux-amd64.tar.gz`
    - 下载macOS pkg一直下一步，默认路径`/usr/local/go`

3. Windows
    - Windows msi默认路径`C:\Go`

### 配置环境变量
1. Linux/macOS/FreeBSD
    - `$ export PATH=$PATH:/usr/local/go/bin`

2. Windows
    - 手动配置环境变量PATH中添加`C:\Go\bin`

3. 检验环境变量
    - `$ go env`指令可查看golang的所有环境变量

### 检测是否安装成功
```bash
 $ go version
 go version go1.11.5 darwin/amd64
```
### 开发工具
1. 集成开发环境IDE
    - Goland：学生用校园邮箱可免费，其他可通过服务器或某宝买个key
    - LiteIDE：开源
    - Visual Studio Code

2. 文本编辑器
    - Vim：Vim-Go插件
    - Atom：Go-plus插件可检查语法和构建错误
    - Sublime Text：GoSublime插件可以自动导包、检查语法错误等

### Hello World!
```Go
package main

import "fmt"

func main() {
	fmt.Printf("hello, world\n")
}
```

```shell
 $ cd \$HOME/go/src/hello
 $ go build
 ./hello
 hello, world
```


----
**注意：升级旧版本需要先卸载旧版本**
### Go多版本管理
1. 安装其他版本在不同路径下，更改环境变量即可
2. docker
公共镜像库：https://hub.docker.com/_/golang?tab=tags
docker公共仓库提供多种Go版本的镜像，可在不同容器中使用不同版本的Go

3. GVM
推荐阅读：https://gocn.vip/article/107

### 卸载Go
1. 删除go的安装目录即可
2. 移除环境变量
    - Linux/FreeBSD：将`~/.profile`中相关配置删除
    - macOS pkg：删除`/etc/paths.d/go`文件
    - Windows：手动删除环境变量
