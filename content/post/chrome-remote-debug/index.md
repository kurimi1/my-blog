---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "Chrome Remote Debug"
subtitle: ""
summary: ""
authors: []
tags: []
categories: []
date: 2022-09-23T06:47:15Z
lastmod: 2022-09-23T06:47:15Z
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

## 启动chrome  remote debug

> ./chrome --remote-debugging-port=9222
图形界面只能监听本地ip，需要端口转发

headless 可以监听所有ip
> --headless --remote-debugging-address=0.0.0.0

## 端口转发

### win10

> netsh interface portproxy add v4tov4 listenport=9222 connectaddress=127.0.0.1 connectport=9222 listenaddress=0.0.0.0

删除转发

> netsh interface portproxy delete v4tov4 listenport=9222 listenaddress=0.0.0.0

注意！，需要开放防火墙端口

### linux

> ssh -L 0.0.0.0:9223:localhost:9222 localhost -N

## 获取ws

访问 /json/version

## 打开chrome调试工具页面

> chrome://inspect

## 获取浏览器访问页面

在headless模式下，访问/json
 