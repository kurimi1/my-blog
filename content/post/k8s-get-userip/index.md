---
title: 在k8s中部署的系统获取用户ip 
summary: 
date: "2021-10-08"
authors:
- admin
tags:
- cloud
- kubernetes
categories:
- 总结
---

## 背景

k8s Cluster模式 访问任何节点ip自带路由跳转(反向代理？)，因此后端获取到的是转换过一次的ip，而不是用户真实ip

## 解决方法

设置：externalTrafficPolicy: Local
