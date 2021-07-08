---
title: 关于使用https的坑
date: "2021-07-07"
authors:
- admin
tags:
- 证书
- https
categories:
- 部署
- 踩坑
- 笔记
---

## x509: cannot validate certificate for x.x.x.x because it doesn't contain any IP SANs 解决方法

这种问题通常出现在ip使用https时(k8s自动生成证书)。因为k8s生成的证书的Subject Alternative Name都是DNS解析的，所以使用nodeport发布服务时node的ip地址不在验证域中，解决方法有两种

### 1：客户端取消证书验证

例如docker 使用insecure-registries配置

### 2：重新生成服务端证书

自签证书被某些客户端 || sdk无法验证，只能采用第一种方式

#### 查看证书（公钥）命令

openssl x509 -in ./public.crt -noout -text

---

在Subject Alternative Name中添加ip

#### 创建一个以openssl.conf以下内容命名的文件。设置IP.1和/或DNS.1指向正确的 IP/DNS 地址

[req]
distinguished_name = req_distinguished_name
x509_extensions = v3_req
prompt = no

[req_distinguished_name]
C = US
ST = VA
L = Somewhere
O = MyOrg
OU = MyOU
CN = MyServerName

[v3_req]
subjectAltName = @alt_names

[alt_names]
IP.1 = 127.0.0.1
DNS.1 = localhost

IP地址不能使用CIDR格式，必须指定到ip

#### openssl通过指定配置文件运行并在出现提示时输入密码

openssl req -new -x509 -nodes -days 730 -key private.key -out public.crt -config openssl.conf

修改完证书后需要重启
