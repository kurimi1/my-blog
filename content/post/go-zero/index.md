---
title: go-zero note
summary: go-zero 笔记
date: "2021-06-23"
authors:
- admin
tags:
- go-zero
- note
- golang
- 微服务框架
categories:
- golang
- 踩坑
- 笔记
---

## config使用环境变量

添加option
>conf.MustLoad(*configFile, &c, conf.UseEnv())
但其他地方不得出现$,会造成误判

## 数据库建立索引会自动生成find代码
