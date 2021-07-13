---
title: redis note
summary: redis 笔记
date: "2021-07-09"
authors:
- admin
tags:
- redis
- note
categories:
- redis
- note
---

## 获取所有key

keys *

## docker 安装redis

持久化
docker run --name some-redis -d -p 6379:6379 redis redis-server --appendonly yes

## 查看key的过期时间

ttl key

## 查看key的类型

type key
