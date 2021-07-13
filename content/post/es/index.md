---
title: es note
summary: es 笔记
date: "2021-07-13"
authors:
- admin
tags:
- es
- note
categories:
- golang
- es
- 笔记
---

## es7中 golang客户端聚合查询

es7 中term无法使用多字段因此使用bool聚合查询
must下匹配，filter进行过滤，range定义范围

## 查询方法

term 精准
match 模糊
match_phrase 模糊但只保留那些包含全部搜索词项，且位置与搜索词项相同的文档
