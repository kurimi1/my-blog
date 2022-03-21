---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "Ubuntu18 Set Ulimit"
subtitle: "ubuntu 18.04 设置ulimit不生效"
summary: ""
authors: [admin]
tags: [linux]
categories: [坑]
date: 2022-03-21T11:45:41+08:00
lastmod: 2022-03-21T11:45:41+08:00
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

## 第一步：修改limit配置文件

```bash
vim /etc/security/limits.conf

# 添加
# 打开文件数不能没限制
* - nofile 65535 
root - nofile 65535
* - nproc 65535
root - nproc 65535
```

*代表所有用户但不包括root，所有root要单独写出来  
切换用户或者重新登录来应用配置  
**通常服务这样配置就可以了，ubuntu20.10可以，但18.04配置不能生效！！！**  
猜测ubuntu LTS版本应该通过系统层面配置

## 第二步：修改系统默认配置文件

```bash
vim /etc/systemd/system.conf 
# 桌面模式或许也要修改user.conf
vim /etc/systemd/user.conf
# 修改
DefaultLimitNOFILE=65535
DefaultLimitNPROC=65535
# 执行
systemctl daemon-reload
```

切换用户或重新登录
