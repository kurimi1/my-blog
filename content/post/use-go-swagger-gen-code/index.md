---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "Use Go Swagger Gen Code"
subtitle: ""
summary: "使用go-swagger生成代码"
authors: [admin]
tags: [openapi]
categories: [教程]
date: 2022-03-09T11:17:59+08:00
lastmod: 2022-03-09T11:17:59+08:00
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

## 项目开发流程

1. 需求
2. 设计
3. 接口设计-swagger
4. http代码生成
5. ent 数据库代码生成，go-zero 数据库代码生成-自带缓存，gorm 数据库代码生成
6. proto grpc代码生成

## 生成api代码的工具对比

1. go-zero生成的api代码，无法定义http code，不符合openapi规范
2. kratos 通过 proto 生成api代码，未实践
3. 编写 swagger 文档，然后用 swagger-codegen 生成代码, 生成的是server stubs
4. 使用 go-swagger生成代码，仅支持2.0，生成完整的服务器代码
5. 使用 openapi-generator 生成代码， 生成的是server stubs，无法直接使用
6. 使用api fox 工具生成，内部使用的是openapi-generator

## openapi 的路径与操作

文档：<https://swagger.io/docs/specification/describing-parameters/#query-parameters>

查询数据库里的唯一字段id，name使用路径path
/users/{id}

查询参数使用query，例如limit，offset等

### openapi-generator

使用apifox 生成，无法直接使用

### go-swagger

### kratos
