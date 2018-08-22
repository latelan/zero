---
layout: post
title: "How to deploy OpenStack with ESXi"
description: ""
date: 2018-08-22
tags: []
comments: true
---

我们使用一台物理机安装ESXi6.5，在上面创建虚机当做物理机模拟真实环境物理机，使用 switch 和 port group 模拟真实网络。

![esxi_openstack](/assets/img/esxi_openstack.png)

如上图所示，以 10.95.42.21 这台物理机做以说明

三个 vSwtich：
* vSwitch0，系统本身创建的 switch，上面有两个端口组（PG：Port Group）
* vSwitch-Mgmt，模拟管理网络，pg-vMgmt 用于管理网络，pg-Mgmt 用于给物理机配置一个管理 IP，可以访问到管理网络。VLAN 都设置0
* vSwitch-Production，模拟业务网络，pg-vProduction 用于业务网，VLAN ID 为4095当做 trunk 口，pg-Production 用于连接 Router，VLAN ID 为200（自定义），当做 access 口

三个 VM：
* ostack01，用于模拟物理机环境，部署 all-in-one 环境，或者添加多个 VM 部署分布式环境，两张网卡，分别连接管理网和业务网
* centos7，充当Router，两张网卡连接管理网和业务网
* win7，当做客户端，连接管理网，用于访问 OpenStack 环境

![esxi_network](/assets/img/esxi_network.png)

网络规划：
Management：192.168.100.0/24
Production: 192.168.200.0/24

注意事项：
* centos7 需要开启 IP 转发，以充当路由器
* win7 可以再添一块网卡连接到 VM Network 端口组，配置物理 IP，使用远程连接访问。
