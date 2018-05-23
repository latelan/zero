---
layout: post
title: "Compile drbd9 on CentOS7"
description: ""
date: 2018-05-23
tags: [Linux,drbd9]
comments: true
---

背景：最近CentOS7系统升级到最新内核后，使用从yum源中安装的drbd9导致内核崩溃

```bash
[root@lms-1 ~]# uname -a
Linux lms-1 3.10.0-862.2.3.el7.x86_64 #1 SMP Wed May 9 18:05:47 UTC 2018 x86_64 x86_64 x86_64 GNU/Linux
[root@lms-1 ~]# yum list installed | grep drbd
drbd90-utils.x86_64                        9.1.0-1.el7.elrepo          @elrepo
kmod-drbd90.x86_64                         9.0.9-1.el7_4.elrepo        @elrepo
```

崩溃信息

```
[  124.792108] drbd r0: Starting worker thread (from drbdsetup [3092])
[  124.795286] ------------[ cut here ]------------
[  124.795873] kernel BUG at lib/nlattr.c:66!
[  124.796362] invalid opcode: 0000 [#1] SMP
[  124.796880] Modules linked in: drbd(OE) ipt_MASQUERADE nf_nat_masquerade_ipv4 iptable_nat nf_conntrack_ipv4 nf_defrag_ipv4 nf_nat_ipv4 nf_nat nf_conntrack iptable_filter veth xfs bridge stp llc dm_thin_pooldm_persistent_data dm_bio_prison dm_bufio libcrc32c loop iosf_mbi crc32_pclmul ghash_clmulni_intel ppdev parport_pc sg aesni_intel lrw gf128mul glue_helper ablk_helper cryptd parport joydev pcspkr virtio_balloon i2c_piix4 ip_tables ext4 mbcache jbd2 sd_mod crc_t10dif crct10dif_generic ata_generic pata_acpi virtio_scsi virtio_net cirrus drm_kms_helper syscopyarea sysfillrect sysimgblt fb_sys_fops ttm drm ata_piix libata serio_raw i2c_core virtio_pci crct10dif_pclmul crct10dif_common virtio_ring crc32c_intel floppy virtio dm_mirror dm_region_hash dm_log dm_mod
[  124.805782] CPU: 1 PID: 3097 Comm: drbdsetup Tainted: G           OE  ------------   3.10.0-862.2.3.el7.x86_64 #1
[  124.806978] Hardware name: Red Hat KVM, BIOS seabios-1.7.5-11.el7 04/01/2014
[  124.807796] task: ffff89f72df4eeb0 ti: ffff89f4b0278000 task.ti: ffff89f4b0278000
[  124.808684] RIP: 0010:[<ffffffff86176a80>]  [<ffffffff86176a80>] validate_nla+0x230/0x240
[  124.809661] RSP: 0018:ffff89f4b027b928  EFLAGS: 00010202
[  124.810284] RAX: 00000000000001f0 RBX: ffff89f778235c34 RCX: 0000000000003720
[  124.811122] RDX: ffffffffc0466f20 RSI: 0000000000000024 RDI: 000000000000000a
[  124.811952] RBP: ffff89f4b027b948 R08: ffffffffc0466f20 R09: ffffffffc04674a0
[  124.815392] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  124.816063] CR2: 00007f6ec5067f40 CR3: 00000000702be000 CR4: 00000000003606e0
[  124.816899] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  124.817726] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  124.818558] Call Trace:
[  124.818859]  [<ffffffff86176bc6>] nla_parse+0xb6/0x120
[  124.819470]  [<ffffffffc044ff53>] drbd_nla_parse_nested+0x43/0x50 [drbd]
[  124.820259]  [<ffffffffc0433c47>] __net_conf_from_attrs.isra.60+0x37/0x940 [drbd]
[  124.821167]  [<ffffffffc044131a>] adm_new_connection+0x9a/0x8e0 [drbd]
[  124.821944]  [<ffffffff86176bc6>] ? nla_parse+0xb6/0x120
[  124.822569]  [<ffffffff86176f24>] ? __nla_reserve_nohdr+0x24/0x30
[  124.823286]  [<ffffffffc0441c10>] drbd_adm_new_peer+0xb0/0xc0 [drbd]
[  124.824035]  [<ffffffff86426c38>] genl_family_rcv_msg+0x208/0x430
[  124.824745]  [<ffffffff86151324>] ? __radix_tree_lookup+0x84/0xf0
[  124.825459]  [<ffffffff86426ebb>] genl_rcv_msg+0x5b/0xc0
[  124.826085]  [<ffffffff864231a0>] ? __netlink_lookup+0xc0/0x110
[  124.826784]  [<ffffffff86426e60>] ? genl_family_rcv_msg+0x430/0x430
[  124.827518]  [<ffffffff86424ecb>] netlink_rcv_skb+0xab/0xc0
[  124.828172]  [<ffffffff86425408>] genl_rcv+0x28/0x40
[  124.828755]  [<ffffffff86424850>] netlink_unicast+0x170/0x210
[  124.829431]  [<ffffffff86424bf8>] netlink_sendmsg+0x308/0x420
[  124.830104]  [<ffffffff863cb84d>] sock_aio_write+0x15d/0x180
[  124.830771]  [<ffffffff85fc527c>] ? handle_pte_fault+0x2dc/0xc30
[  124.831471]  [<ffffffff8601a193>] do_sync_write+0x93/0xe0
[  124.832105]  [<ffffffff8601ad75>] vfs_write+0x1c5/0x1f0
[  124.832717]  [<ffffffff8601ba9f>] SyS_write+0x7f/0xf0
[  124.833314]  [<ffffffff8651f795>] system_call_fastpath+0x1c/0x21
[  124.834014] Code: 5e 0f 4f c2 5d c3 0f 1f 44 00 00 31 d2 66 85 c9 74 de e9 e4 fe ff ff b8 ea ff ff ff e9 3c fe ff ff b8 de ff ff ff e9 32 fe ff ff <0f> 0b 0f 1f 40 00 66 2e 0f 1f 84 00 00 00 00 00 55 83 fe 03 48
[  124.837228] RIP  [<ffffffff86176a80>] validate_nla+0x230/0x240
[  124.837925]  RSP <ffff89f4b027b928>
[  124.838356] ---[ end trace 43a47828d486244d ]---
[  124.838903] Kernel panic - not syncing: Fatal exception
[  124.839568] Kernel Offset: 0x4e00000 from 0xffffffff81000000 (relocation range: 0xffffffff80000000-0xffffffffbfffffff)
```

随后决定自己编译安装，下面是制作rpm包的过程

编译环境

```
OS：CentOS7
Kernel ：3.10.0-862.2.3
```

下载软件包

<https://www.linbit.com/en/drbd-community/drbd-download/>
[drbd-9.0.14-1.tar](http://www.linbit.com/downloads/drbd/9.0/drbd-9.0.14-1.tar.gz)
[drbd-utils-9.4.0.tar](http://www.linbit.com/downloads/drbd/utils/drbd-utils-9.4.0.tar.gz)

升级内核

```
yum install kernel kernel-devel kernel-headers
```

安装编译需要的工具

```
yum install gcc make automake gcc-c++ flex
```

编译drbd内核模块

```bash
tar xzvf drbd-9.0.14-1.tar
cd /drbd-9.0.14-1
make
...
    Module build was successful.
=======================================================================
  With DRBD module version 8.4.5, we split out the management tools
  into their own repository at https://github.com/LINBIT/drbd-utils
  (tarball at http://links.linbit.com/drbd-download)

  That started out as "drbd-utils version 8.9.0",
  has a different release cycle,
  and provides compatible drbdadm, drbdsetup and drbdmeta tools
  for DRBD module versions 8.3, 8.4 and 9.

  Again: to manage DRBD 9 kernel modules and above,
  you want drbd-utils >= 9.3 from above url.
=======================================================================
```

制作rpm包

```bash
make kmp-rpm
```



编译drbd-utils

```bash
tar xzvf drbd-utils-9.4.0.tar
cd drbd-utils-9.4.0
直接生成drbd.spec文件，制作rpm包，因为源码bug的关系，需要手动建立/root/rpmbuild/SOURCES目录
mkdir /root/rpmbuild/SOURCES
生成Makefile文件
./configure --prefix=/usr --localstatedir=/var --sysconfdir=/etc --without-83support --without-84support
生成drbd.spec文件
./configure --prefix=/usr --localstatedir=/var --sysconfdir=/etc --without-83support --without-84support --enable-spec
make rpm
...
You have now:
/root/rpmbuild/RPMS/x86_64/drbd-9.4.0-1.el7.centos.x86_64.rpm
/root/rpmbuild/RPMS/x86_64/drbd-utils-9.4.0-1.el7.centos.x86_64.rpm
/root/rpmbuild/RPMS/x86_64/drbd-xen-9.4.0-1.el7.centos.x86_64.rpm
/root/rpmbuild/RPMS/x86_64/drbd-udev-9.4.0-1.el7.centos.x86_64.rpm
/root/rpmbuild/RPMS/x86_64/drbd-pacemaker-9.4.0-1.el7.centos.x86_64.rpm
/root/rpmbuild/RPMS/x86_64/drbd-heartbeat-9.4.0-1.el7.centos.x86_64.rpm
/root/rpmbuild/RPMS/x86_64/drbd-bash-completion-9.4.0-1.el7.centos.x86_64.rpm
/root/rpmbuild/RPMS/x86_64/drbd-man-ja-9.4.0-1.el7.centos.x86_64.rpm
/root/rpmbuild/RPMS/x86_64/drbd-debuginfo-9.4.0-1.el7.centos.x86_64.rpm
```

这样编译出来的包安装会可能会出现以下问题：

```bash
[root@lms-0 ~]#  rpm -ivh drbd-utils-9.4.0-1.el7.centos.x86_64.rpm
Preparing...                          ################################# [100%]
        file /usr/sbin/drbdadm conflicts between attempted installs of drbd-utils-9.4.0-1.el7.centos.x86_64 and drbd-utils-9.4.0-1.el7.centos.x86_64
        file /usr/sbin/drbdmeta conflicts between attempted installs of drbd-utils-9.4.0-1.el7.centos.x86_64 and drbd-utils-9.4.0-1.el7.centos.x86_64
        file /usr/sbin/drbdsetup conflicts between attempted installs of drbd-utils-9.4.0-1.el7.centos.x86_64 and drbd-utils-9.4.0-1.el7.centos.x86_64
```

解决办法：修改drbd.spec.in文件

```text
  # conditionals may not contain "-" nor "_", hence "bashcompletion"
  %bcond_without bashcompletion
  %bcond_without sbinsymlinks
+ %if %{rhel} == 7
+ %undefine with_sbinsymlinks
+ %endif
  # --with xen is ignored on any non-x86 architecture
  %bcond_without xen
  %bcond_without 83support
```

重新打包，安装成功

参考

<http://tldp.org/HOWTO/RPM-HOWTO/build.html>
<http://fedoraproject.org/wiki/Packaging:RPMMacros>
<https://docs.linbit.com/doc/users-guide-84/ch-build-install-from-source/>
<http://lists.linbit.com/pipermail/drbd-user/2015-November/021922.html>
