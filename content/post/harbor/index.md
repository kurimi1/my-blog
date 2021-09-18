---
title: 使用helm安装harbor
summary: helm安装harbor教程和遇见的坑
date: "2021-06-11"
authors:
- admin
tags:
- helm
- harbor
categories:
- 教程
- 踩坑
---
## helm下载harbor

helm repo add harbor <https://helm.goharbor.io>
helm fetch harbor/harbor --untar
helm pull harbor/harbor

## 配置（主要地方）

编辑values.yaml

### 服务方式坑点

服务方式：ingress,nodePort
选择ingress需要配置域名服务器，否则需要配置每台机器的hosts。
最主要的是！！！drone ci 的docker plugins无法配置hosts
因此强烈建议内网部署采用nodePort方式()

>type: nodePort

### http or https 坑点

使用nodeport无法使用https,登录会报证书无法验证
>x509: cannot validate certificate for 10.67.15.211 because it doesn't contain any IP SANs

### k8s登录坑点

因为是自签证书所以所有节点的docker都要配置insecure-registries,否则无法pull镜像,ingress方式或nodeport都需要配置
for example：
>"insecure-registries": ["harbor.com", "10.67.15.211:30003"]

docker默认配置针对127.0.0.0/8本地回环地址不验证，但还有10.0.0.0/8这个内网

### externalURL

协议://ip：端口

### pvc配置

提供storageClass即可

## 配置docker http登录

vim /etc/docker/daemon.json

添加haobor地址
>"insecure-registries": ["10.67.15.211:30005"]

systemctl daemon-reload

systemctl restart docker
