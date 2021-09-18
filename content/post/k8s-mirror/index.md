---
title: 解决k8s 国外镜像下载问题 
summary: 一次性解决所有仓库 
date: "2021-09-14"
authors:
- admin
tags:
- k8s
categories:
- 教程
- 踩坑
---

## 问题描述

所有国内镜像库都不可用

## 解决方法-设置代理

docker 为service，配置代理方法如下
> 注意：将国内库与私有库设置为no_proxy

### docker 配置代理

sudo mkdir -p /etc/systemd/system/docker.service.d

cat <<EOF | sudo tee /etc/systemd/system/docker.service.d/http-proxy.conf
[Service]
Environment="HTTP_PROXY=socks5://10.67.15.212:1080"
Environment="HTTPS_PROXY=socks5://10.67.15.212:1080"
Environment="NO_PROXY=localhost,127.0.0.1,docker.mirrors.ustc.edu.cn,docker.io,docker.com,registry.cn-hangzhou.aliyuncs.com,core.harbor.intra.com,10.67.15.211:30003"
EOF

sudo systemctl daemon-reload
sudo systemctl restart docker

#### 踩坑

这两个官方域名也不能代理docker.io,docker.com
