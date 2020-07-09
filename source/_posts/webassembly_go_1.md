---
title: 使用go编写webassembly
date: 2019-12-10 12:34:10
tags: 
- go
categories:
 - 学习
---
# 使用go编写webassembly并在浏览器执行
# 参考博客
[用go写WebAssembly入门](https://www.cnblogs.com/baizx/p/9602581.html)
## 下载安装
[go](https://www.runoob.com/go/go-environment.html)
## 编写测试文件hello.go
```
package main

import "fmt"

func main() {
        fmt.Println("Hello World!")
}
```
## 生成wasm文件
``` GOARCH=wasm GOOS=js go build -o hello.wasm hello.go```
在windows下需要先设置环境变量：``` $env:GOARCH="wasm";$env:GOOS="js"; ```
## 添加依赖
``` 
cp $(go env GOROOT)/misc/wasm/wasm_exec.{html,js} . 
```
## 添加一个测试http服务器
``` 
//http.go
package main

import (
    "flag"
    "log"
    "net/http"
    "strings"
)

var (
    listen = flag.String("listen", ":8080", "listen address")
    dir    = flag.String("dir", ".", "directory to serve")
)

func main() {
    flag.Parse()
    log.Printf("listening on %q...", *listen)
    log.Fatal(http.ListenAndServe(*listen, http.HandlerFunc(func(resp http.ResponseWriter, req *http.Request) {
        if strings.HasSuffix(req.URL.Path, ".wasm") {
            resp.Header().Set("content-type", "application/wasm")
        }

        http.FileServer(http.Dir(*dir)).ServeHTTP(resp, req)
    })))
}
```
## 执行
go run http.go
## 查看效果
浏览器打开http://localhost:8080/wasm_exec.html，点击run，控制台可以看到效果
## node执行
``` 
node wasm_exec.js test.wasm
```
