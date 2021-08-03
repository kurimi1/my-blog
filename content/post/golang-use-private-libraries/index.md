---
title: goalng使用gitlab私有库
summary: 
date: "2021-08-03"
authors:
- admin
tags:
- 教程
- 踩坑
categories:
- golang
---

## 设置私有库

> go env -w GOPRIVATE="gitlab.com"

## 如果内网gitlab用的http需设置

> go env -w GOINSECURE="gitlab.com"

## 坑点：被导入的私有库的mod必须包含gitlib

> module gitlab.com/xxx
