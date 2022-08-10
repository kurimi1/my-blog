---
title: go test
summary: 单元测试，基准测试
date: "2021-07-08"
authors:
- admin
tags:
- golang
- test
- note
categories:
- golang
- note
---

## 指定测试函数

> go test -v -test.run xxxx

使用正则指定

go test -run ^xxx&

## 基准测试

> go test -benchmem -run=^& -bench ^xxx$
必须使用-run=^&才能指定测试某个基准测试，否在单测基测都会执行

## 并发基准测试

```go
b.Setparallelism(2) // 设置并发数量2x核心数
b.Runparallel(func(pb *testing.PB) {
    for pb.Next() {
        xx()
    }
})
```
