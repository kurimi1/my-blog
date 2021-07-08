---
title: vscode设置proto插件
summary: 高亮，格式化
date: "2021-06-21"
authors:
- admin
tags:
- vscode
- grpc
categories:
- proto
- vscode
---

## 安装插件

搜索安装这两个插件，第一个插件会调用第二个插件进行格式化

- vscode-proto3
- clang-format

## 插件配置

"clang-format.style": "google"
"clang-format.style": "{ IndentWidth: 4, BasedOnStyle: google, AlignConsecutiveAssignments: true }"
