---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "Go Pprof Note"
subtitle: ""
summary: ""
authors: [admin]
tags: [pprof]
categories: [go]
date: 2022-07-21T02:30:58Z
lastmod: 2022-07-21T02:30:58Z
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---
## 作用

1. 性能分析
2. 内存泄露

## 只运行一次

文档<https://pkg.go.dev/runtime/pprof>

### 使用test bench

go test -cpuprofile cpu.prof -memprofile mem.prof -bench .

### 加入写文件代码

```go
var cpuprofile = flag.String("cpuprofile", "", "write cpu profile to `file`")
var memprofile = flag.String("memprofile", "", "write memory profile to `file`")

func main() {
    flag.Parse()
    if *cpuprofile != "" {
        f, err := os.Create(*cpuprofile)
        if err != nil {
            log.Fatal("could not create CPU profile: ", err)
        }
        defer f.Close() // error handling omitted for example
        if err := pprof.StartCPUProfile(f); err != nil {
            log.Fatal("could not start CPU profile: ", err)
        }
        defer pprof.StopCPUProfile()
    }

    // ... rest of the program ...

    if *memprofile != "" {
        f, err := os.Create(*memprofile)
        if err != nil {
            log.Fatal("could not create memory profile: ", err)
        }
        defer f.Close() // error handling omitted for example
        runtime.GC() // get up-to-date statistics
        if err := pprof.WriteHeapProfile(f); err != nil {
            log.Fatal("could not write memory profile: ", err)
        }
    }
}
```

## 持续运行

文档<https://pkg.go.dev/net/http/pprof>

添加以下代码

```go
import _ "net/http/pprof"
go func() {
	log.Println(http.ListenAndServe("localhost:6060", nil))
}()
```

地址：<http://localhost:6060/debug/pprof>

## 下载pprof文件

直接go tool pprof <http://localhost:6060/debug/pprof/heap>,会自动下载文件
cpu是/profile,默认采集30s内的

curl -s /xxx > xx 下载

直接通过web查看

>go tool pprof -http=[host]:[port] [options] <http://localhost:6060/debug/pprof/heap>

## pprof命令行

官方文档<https://github.com/google/pprof/blob/main/doc/README.md>
中文文档<https://github.com/hyper0x/go_command_tutorial/blob/master/0.12.md>

### 开启web

> go tool pprof -http=[host]:[port] [options] source

## 磁盘分析，网络分析

目前没看到开源项目，分析程序内部磁盘和网络情况

## bench

go test -bench -cpuprofile xxx

## --base 参数2个profile对比

--base x y

## 内存逃逸分析

通过pprof确认内存泄露代码位置，根据以下逃逸类型进行分析。逃逸后会被gc回收，但影响运行速度！

### 编译静态分析

> -gcflags "-m" -l关闭内联

### 指针逃逸

函数返回局部变量指针，则局部变量被分配到堆上。
准则：除了大结构体，make，new，其他变量一律不使用指针。

### 栈空间不足

就算make，new没有回传指针，但超过系统栈空间也会分配到堆上。小于栈则分配到栈上，但如果返回指针则分配到堆上。当不确定大小时，分配到堆。给出cap分配到栈可以提高性能！
ulimit -s 查看栈大小

### interface 动态类型逃逸

fmt.Println 会使变量逃逸，但println 不会

### 闭包引用逃逸

闭包：一个函数和其周围状态的引用捆绑在一起，这样的组合就是闭包。也就是说，闭包让你可以在一个内层函数中访问到其外层函数的作用域。

```go
func Increase() func() int {
    n := 0
    return func() int {
        n++
        return n
    }
}

func main() {
    in := Increase()
    println(in()) // 1
    println(in()) // 2
}
```
