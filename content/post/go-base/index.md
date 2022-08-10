---
title: golang的一些笔记
summary: golang笔记
date: "2021-07-22"
authors:
- admin
tags:
- note
- course
- pitfall
categories:
- golang
- note
---

## 多态的方法

1. 常用的interface接口
2. 利用反射reflect判断struct的name(或者直接获取xxx.(type))

## 限制并发

分为以下2个方面，最佳实践为协程数量>并发数量，通常起多少个协程就代表进行多少并发，因为go本身还有其他协程，因此协程数>并发数。

### 限制创建协程数量

limiter在协程创建之前

```go
limiter <- struct{}{}
go func
```

### 限制协程并发数量

limiter在协程之中

```go
go func () {
    limiter <- struct{}{}
}
```

### 释放limiter

建议使用defer释放，当协程panic后保障释放

```go
go func () {
    defer func (){
        <-limiter
    }
}
```

## waitgroup

wg.add()必须在协程前添加

```go
wg.Add(1)
go func () 
```

如果在协程能add，会造成协程还没add，wait就先执行就直接退出了

## 从chan中动态起协程，仅适用于不需要退出的场景

不能使用waitgroup，因为协程是动态增减的，会在某个瞬间为0
for chan里不能使用wg

优点：不需要初始化协程数量，节约内存
缺点：无法使用wg，无法确定所有协程结束时间

## pcre正则比标准库快1倍多

rust nim最快，可以使用cgo调用

## 程序并发设置

如果是多任务，直接给任务设置并发，任务内没必要再并发
