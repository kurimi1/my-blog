---
title: go控制协程并发数量
summary: 方式比较到最佳实践
date: "2021-11-3"
authors:
- admin
tags:
- go
categories:
- go
---

## 方法

1. 利用channel
2. 第三方库ants，协程池
3. 官方包sync/x/Semaphore

## 总结

协程池可以复用，节约内存，可以动态调整大小。

channel限制并发，可以动态改变吗？
