---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "Filebeat Config on Insecure Https"
subtitle: "filebeat如何链接不安全https的ES"
summary: ""
authors: []
tags: [es]
categories: [坑]
date: 2022-03-16T17:12:32+08:00
lastmod: 2022-03-16T17:12:32+08:00
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

## 官方文档的坑

本来开开心心按照官方文档配置，结果发现https一波三折啊。

官方文档：<https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-installation-configuration.html>

当配置ca_trusted_fingerprint时，需要去es docker上执行
> openssl x509 -fingerprint -sha256 -in config/certs/http_ca.crt

结果生成hash指纹是：H9:7F:S8 这种格式，这事第一个大坑。需要把：删除字母转换为小写，放到ca_trusted_fingerprint里面。

当我以为这就万事大吉了，结果发现还行链接不上,报错：
>{"log.level":"error","@timestamp":"2022-03-16T08:45:25.788Z","log.logger":"esclientleg","log.origin":{"file.name":"transport/logging.go","file.line":37},"message":"Error dialing x509: certificate is valid for localhost, a0e3183bf337, not elasticsearch","service.name":"filebeat","network":"tcp","address":"elasticsearch:9200","ecs.version":"1.6.0"}

ES自动生成的证书，只能用于本地，不能用于远程。也就是说，证书签名的域名为localhost，filebeat只能通过localhost访问es。但我使用的docker，只能通过远程访问。此时陷入了胶着。

然后，我就发现了ES官网里的一个提问。
<https://discuss.elastic.co/t/merticbeat-error-dialing-x509-certificate-is-valid-for-127-0-0-1-not-172-16-0-204/299323>

可以通过verification_mode: none to disable hostname checking.<https://www.elastic.co/guide/en/beats/filebeat/current/ssl-client-fails.html>

然后看到了ssl的配置文档：<https://www.elastic.co/guide/en/beats/filebeat/current/configuration-ssl.html#client-verification-mode>
可以设置verification_mode:
full
>验证提供的证书是否由受信任的机构 (CA) 签名，并验证服务器的主机名（或 IP 地址）是否与证书中标识的名称匹配。

strict

>验证提供的证书是否由受信任的机构 (CA) 签名，并验证服务器的主机名（或 IP 地址）是否与证书中标识的名称匹配。如果主题备用名称为空，则返回错误。

certificate

>验证提供的证书是否由受信任的权威 (CA) 签名，但不执行任何主机名验证。

none

不 执行服务器证书的验证。此模式会禁用 SSL/TLS 的许多安全优势，并且只能在谨慎考虑后使用。它主要用作尝试解决 TLS 错误时的临时诊断机制；强烈建议不要在生产环境中使用它。

默认值为full。
