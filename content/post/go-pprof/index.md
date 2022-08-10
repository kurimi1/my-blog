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

## pprof命令行

官方文档<https://github.com/google/pprof/blob/main/doc/README.md>
中文文档<https://github.com/hyper0x/go_command_tutorial/blob/master/0.12.md>

### 开启web

> go tool pprof -http=[host]:[port] [options] source

## 磁盘分析，网络分析

目前没看到开源项目，分析程序内部磁盘和网络情况

## bench

go test -bench -cpuprofile xxx
