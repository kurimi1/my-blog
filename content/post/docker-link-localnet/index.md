---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "Docker Link Localnet"
subtitle: ""
summary: "docker 访问本地网络"
authors: [admin]
tags: [docker]
categories: [notes]
date: 2022-03-04T18:21:45+08:00
lastmod: 2022-03-04T18:21:45+08:00
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

## 设置net为host

> docker run --net host

可直接访问

## 使用默认的bridge

> docker network inspect bridge
> 
查看网关，通过网关访问本地网络
