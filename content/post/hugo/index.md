---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "Hugo Notes"
subtitle: "hugo"
summary: "简单记录 hugo 使用笔记"
authors: [admin]
tags: [hugo]
categories: [notes]
date: 2022-03-07T11:29:57+08:00
lastmod: 2022-03-07T11:29:57+08:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: "Center"
  preview_only: false
  placement: 2

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

## 下载

有些主题可能不支持，一定要下载extended版本。

>go install --tags extended github.com/gohugoio/hugo@latest

## 本地预览命令

> hugo server --bind 0.0.0.0 -D

-D 代表编译草稿

## 创建post

> hugo new --kind post post/my-article-name

## 编译命令

> hugo

## 发布

push 到 github
