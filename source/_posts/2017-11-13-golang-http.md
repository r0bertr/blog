---
title: Golang net/http 源码分析
date: 2017-11-13
tags:
    - 中文
    - golang
---

Go语言的内置包`net`提供了大量api，功能十分强大、实现非常优美，不读一读实在是有点可惜呀。

## 程序

首先，写一个简单的服务器。

    package main

    import (
        "fmt"
        "log"
        "net/http"
        "strings"
    )

    func main() {
        // A simple http server.
        //
        // This server send "Hello user" to client based on URL.
        // Example:
        //
        // $ curl localhost:8080/foo/bar
        // > Hello foo bar!
        //
        http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
            fmt.Fprintf(w, "Hello %s!\n",
    		      strings.Join(strings.Split(r.URL.String(), "/")[1:], " "))
        })

        fmt.Println("Listening to port 8080.")
        log.Fatal(http.ListenAndServe(":8080", nil))
    }

如注释所说，这个服务器会发一个“Hello xxx”给客户端，一个简单的hello world式程序。

## 代码分析

虽然这个服务器只有不到10行，但是通过代码追踪（ctrl+鼠标左键），我们可以一层一层地看到`net/http`包里各种各样的接口。

### `http.HandleFunc`

    http.HandleFunc(pattern string, handler func(ResponseWriter, *Request))
    -> (mux *ServeMux) HandleFunc(pattern string, handler func(ResponseWriter, *Request))
    -> (mux *ServeMux) Handle(pattern string, handler Handler)

- `http`包中的`HandleFunc`方法会给默认的`ServerMux`注册一个`Handler`，用于响应客户端发来的请求；它包含两个参数，一个是字符串类型的`URL`，另一个是`Handler`函数，这两个参数经过三层传递传递到`DefaultServeMux.Handle`函数中，才会被正式执行注册操作。
- `ServerMux`是一个HTTP协议请求复用器，其中包含多个`URL`到`Handler`的映射，用于匹配不同的客户端请求，并执行相应的`Handler`；`ServerMux`支持近似匹配，当匹配不完全时，它会寻找最接近的匹配。
- `DefaultServerMux`是包中自带的默认复用器，也就是说，开发者可以定义自己的复用器。
- `Handler`是一个接口，包含一个处理函数。

这个过程涉及到两个重要结构`ServeMux`和`muxEntry`。

    // type ServeMux
    type ServeMux struct {
        mu    sync.RWMutex
        m     map[string]muxEntry
        hosts bool // whether any patterns contain hostnames
    }
    // type muxEntry
    type muxEntry struct {
        explicit bool
        h        Handler
        pattern  string
    }

- `mu` 一个读写排它锁，用于保证注册`Handler`过程的原子性。（操作系统知识怎么在这出现了）
- `m` 映射，储存了从`URL`到`muxEntry`的映射，`muxEntry`中包含了处理函数`Handler`。
- `hosts` 表示是否有某个`pattern`包含主机名。

### `http.ListenAndServe`

    http.ListenAndServe(addr string, handler Handler)
    -> (srv *Server) ListenAndServe()
    -> (srv *Server) Serve(l net.Listener)

- `http.ListenAndServe`方法创建一个`Server`，监听TCP地址`addr`并使用`handler`来处理接收到的请求。
- `Server`定义了运行一个HTTP服务器的各种参数，包括TCP地址、`Handler`、TLS参数、超时时间、最大头长度等等。
- `Server`的`ListenAndServe`方法创建一个`net`包中的传输层TCP监听器，并调用`Serve`方法。`Serve`方法接收一个TCP监听器，通过该监听器获得连接信息，并为每一个连接创建一个线程并调用`Server`的`Handler`来响应。具体实现细节涉及网络编程。
