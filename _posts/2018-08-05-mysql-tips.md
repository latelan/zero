---
layout: post
title: "MySQL Tips"
description: ""
date: 2018-08-05
tags: [mysql]
comments: true
---

* 如何让 mysql cli 自动补全 tab 键和 column 信息
在 /etc/my.cnf 中加入一下配置
```text
[mysql]
auto-rehash
```

* 如何使用 Ctrl+w 删除一个单词而不是光标前的所有内容，使用 Ctrl+u 删除光标前所有内容
在 $HOME 下创建 .editrc 文件，配置如下内容
```text
bind "^W" ed-delete-prev-word
bind "^U" vi-kill-line-prev
```

参考链接
1. [https://dev.mysql.com/doc/refman/5.6/en/mysql-tips.html]()
