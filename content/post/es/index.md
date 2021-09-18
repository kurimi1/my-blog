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

## docker 部署的ES无法探测

解决方法：
set false

> client, err = elastic.NewClient(elastic.SetSniff(false), elastic.SetURL(es_addr))

## eck 安装es，取消安全选项

### eck需要配置

setDefaultSecurityContext: false

### es 需要配置

config:
      xpack.security.enabled: false
      xpack.security.http.ssl.enabled: false
      xpack.security.transport.ssl.enabled: false
      xpack.security.authc.reserved_realm.enabled: false

### 其他配置

取消就绪检测等
