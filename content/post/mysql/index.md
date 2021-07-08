---
title: mysql
summary: mysql 笔记
date: "2021-06-21"
authors:
- admin
tags:
- mysql
- note
categories:
- 数据库
- 踩坑
- 笔记
---

## mysql修改密码

>ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '在这里输入你的密码';

## mysql 创建用户及授权

>create user 'test'@'%' identified with mysql_native_password BY '密码';
>grant all privileges on *.* to 'test1'@'localhost' with grant option;

更新权限
>flush privileges;

## mysql 开启命令补全

>mysql -u root -p --auto-rehash
