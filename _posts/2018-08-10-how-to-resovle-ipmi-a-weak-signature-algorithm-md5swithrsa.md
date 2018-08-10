---
layout: post
title: "如何解决 IPMI MD5withRSA被视为弱签名无法访问的问题"
description: ""
date: 2018-08-10
tags: [linux]
comments: true
---

安装了 java 1.8 环境访问IPMI遇到以下问题，一般把IP加入了例外列表就能够访问。但这种情况不行。
![ipmi_error_1](/assets/img/ipmi_error_1.png) ![ipmi_error_2](/assets/img/ipmi_error_2.png)

解决办法有两种：
1. 卸掉 java 1.8，安装1.7版本
2. 修改 java.security 文件

记录下第二种方法。找到 java 安装路径中的 java.security 文件，用记事本管理员权限打开，注释掉如下参数，然后就能使用IMPI了。

```
jdk.jar.disabledAlgorithms=MD2, MD5, RSA keySize < 1024, DSA keySize < 1024
#jdk.jar.disabledAlgorithms=MD2, MD5, RSA keySize < 1024, DSA keySize < 1024
```

