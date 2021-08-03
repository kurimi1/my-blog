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
- mysql
---

## 好的总结

<https://blog.csdn.net/guorui_java/article/details/118558095>

## mysql修改密码

>ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '在这里输入你的密码';

## mysql 创建用户及授权

>create user 'test'@'%' identified with mysql_native_password BY '密码';
>grant all privileges on *.* to 'test1'@'localhost' with grant option;

更新权限
>flush privileges;

## mysql 开启命令补全

>mysql -u root -p --auto-rehash

## 修改配置文件，把127.0.0.1注释掉

vim /etc/mysql/mysql.conf.d/mysqld.cnf

## varchar

同样的，varchar(M)中的的M表示保存的最大字符数，单个字母、数字、中文等都是占用一个字符。varchar可存储的长度范围为0-65535字节，此外，varchar需要使用1或者2个额外字节记录字符串的长度：如果列的最大长度小于或等于255字节，则只使用1个字节表示，否则使用2个字节。对于Innodb引擎，utf8字符集来说，单个中文字符占用3个字节，所以varchar(M)中的M最大不会超过21845，即M的范围是[0,21845)，并且M必须指定。另外MySQL规定：单个字段长度不大于65535字节；单行最大限制为65535，这里不包括TEXT、BLOB字段。即单张表中的所有varchar字段定义的长度之和不能大于65535，所以并不是所有varchar(M)字段中的M都可以取到21844

## mysql 8

### 约束字段为unique唯一时，会自动建立索引
