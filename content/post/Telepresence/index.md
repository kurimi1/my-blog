---
title: 使用Telepresence本地开发调试k8s微服务
summary: 本地开发调试k8s微服务
date: "2021-07-16"
authors:
- admin
tags:
- note
- course
- pitfall
categories:
- k8s
---

## 介绍

借助Telepresence实现以下：

- 本地服务可以访问k8s集群内的服务
- 别人访问k8s集群内的服务，请求可以转发到我本地，从而能够debug

## 安装

官方文档
<https://www.telepresence.io/docs/latest/howtos/intercepts/>

## 配置

```bash
mkdir -p ~/.kube/config
# 将master的kubeconfig拷贝到~/.kube/config/
vim config
```

## 踩坑

因为是本地开发，所以需要设置一下环境变量，否则会报错
设置hosts   k8s API server的IP地址

```bash
echo "10.254.0.1 kubernetes" >> /etc/hosts
```

## 链接

telepresence connect

## 断开

telepresence quit

## bug

终端链接数据库，会断
