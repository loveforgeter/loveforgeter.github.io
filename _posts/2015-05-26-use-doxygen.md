---
layout: post
title: "使用doxygen生成文档"
tags: coding doc
categories: coding
type: origin
---

> 系统：ubuntu 14.04

1. 安装doxygen
--------------
```sh
apt-get install doxygen
```

2. 生成配置文件
--------------
```sh
doxygen -g cfg
```

3. 修改配置文件
---------------
```objc
Project_Name = "Your Project Name"
OUTPUT_DIRECTORY = "./Doxygen"
// 生成调用关系图
CALL_GRAPH = YES
CALLER_GRAPH = YES
EXTRACT_ALL = YES
EXTRACT_STATIC = YES
```

4. 生成文档
-----------
在源代码目录下执行
```sh
doxygen cfg
```
