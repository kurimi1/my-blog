---
title: protobuf note
summary: protobuf 笔记
date: "2021-06-22"
authors:
- admin
tags:
- proto
- note
categories:
- 序列化
- 踩坑
- 笔记
---

## proto3定义数组类型

repeated允许字段重复，对于Go语言来说，它会编译成数组(slice of type)类型的格式。
>repeated string Urls = 5;

## proto3返回struct数组

message foo {
    string Title         = 1;
    int64 StatusCode     = 2;
    int64 Size           = 3;
    string Survey        = 4;
    repeated string Urls = 5;
    string ScreenShot    = 6;
}

// 返回域名相关情报
message boo {
    repeated foo results = 1;
}
