---
layout: post
title: "在Centos上使用httpd"
description: ""
date: 2015-04-07
tags: [linux]
---

最近在win7上使用virtualbox建了个centos6的虚拟机，搭建httpd服务做开发用。由于公司网络限制，虚机只能用nat。为了能在win7上访问虚机的httpd，建立了端口映射，好了前面都没有问题。可就是访问不了，这时就要看下虚机的防火墙问题，方便起见，把iptables关掉。

> $chkconfig --level 35 iptables off

重启，ok
