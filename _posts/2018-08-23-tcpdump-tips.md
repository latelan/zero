---
layout: post
title: "tcpdump tips"
description: ""
date: 2018-08-23
tags: []
comments: true
---

在服务器上探测网卡对应的交换机端口
tcpdump -i eth0 ether proto 0x88cc -A -c 1
``` bash
[root@ostack01 ~(keystone_admin)]# tcpdump -i enp7s0f1 ether proto 0x88cc -A -c 1
listening on enp7s0f1, link-type EN10MB (Ethernet), capture size 65535 bytes
21:29:54.600928 LLDP, length 342: N-3_2_10-13-QA-H58-10.18.97.5
...t%...%...GigabitEthernet1/0/25...x..GigabitEthernet1/0/25 Interface
.N-3_2_10-13-QA-H58-10.18.97.5..H3C Comware Platform Software, Software Version 5.20 Release 1118
H3C S5830-52SC
Copyright (c) 2004-2013 Hangzhou H3C Tech. Co., Ltd. All rights reserved..............1................1        VLAN 0817.      .....l.............    ...............$...
```

查看 VLAN ID 加上 -e 参数
tcpdump -nn -ni eth0 -e

抓 ARP 包
tcpdump -nn -ni eth0 arp
