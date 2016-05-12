---
layout: post
title: "vboxmanage命令使用"
description: ""
category: 
tags: []
---
{% include JB/setup %}

使用virtual box管理本地的虚拟机，经常是以后台方式运行，挂载U盘什么的需要些命令，整理如下。  
1.后台启动ubuntu1404  

	vboxmanage startvm ubuntu1404 -type vrdp  

2.休眠

	vboxmanage controlvm ubuntu1404 savestate

3.挂载U盘  
列出当前宿主机上的U盘信息

	vboxmanage list usbhost

挂载到ubuntu1404
	
	vboxmanage controlvm ubuntu1404 usbattach <uuid>

卸载

	vboxmanage controlvm ubuntu1404 usbdetach <uuid>
