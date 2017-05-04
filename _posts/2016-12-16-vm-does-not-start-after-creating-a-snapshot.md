---
layout: post
title: "Virtualbox: VM does not start after creating a snapshot"
description: ""
date: 2016-12-16
tags: [Linux, Virtualbox]
---

在创建一个快照后，虚机无法启动，并报错

> 返回 代码:E_FAIL (0x80004005)  
> 组件:MediumWrap  
> 界面:IMedium {4afe423b-43e0-e9d0-82e8-ceb307940dda}  

大概意思说这个快照的Parent UUID和注册的存储硬盘ID不一致，导致虚机无法启动。

怎么解决？需要正确设置这个快照的Parent UUID。

查看下存储硬盘的信息，

```
[C:\...]$ vboxmanage internalcommands dumphdinfo ubuntu1404.vhd
--- Dumping VD Disk, Images=1
Dumping VD image "ubuntu1404.vhd" (Backend=VHD)
Header: Geometry PCHS=26398/16/255 LCHS=0/0/0 cbSector=512
Header: uuidCreation={60971c69-2213-4089-8601-2af3aeae7c7a}
Header: uuidParent={00000000-0000-0000-0000-000000000000}
```

查看snapshot的信息，

```
[C:\...]$ vboxmanage internalcommands dumphdinfo {02e3d897-68a1-4810-a64e-1e104c0fc431}.vhd
--- Dumping VD Disk, Images=1
Dumping VD image "{02e3d897-68a1-4810-a64e-1e104c0fc431}.vhd" (Backend=VHD)
Header: Geometry PCHS=26398/16/255 LCHS=0/0/0 cbSector=512
Header: uuidCreation={02e3d897-68a1-4810-a64e-1e104c0fc431}
Header: uuidParent={00000000-0000-0000-0000-000000000000}
```
uuidParent={00000000-0000-0000-0000-000000000000}不正确，设置一下。

```
[C:\...]$ vboxmanage internalcommands sethdparentuuid {02e3d897-68a1-4810-a64e-1e104c0fc431}.vhd 60971c69-2213-4089-8601-2af3aeae7c7a
UUID changed to: 60971c69-2213-4089-8601-2af3aeae7c7a
```

这时候虚机可以正常启动了。
