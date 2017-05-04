---
layout: post
title: "Ubuntu12.04配置msmtp和mutt发送邮件"
description: ""
date: 2014-07-13
tags: [Linux]
---

linux下发送邮件可以使用mail，看见mutt是轻量级的，决定试试。利用邮件功能发送提示信息。使用外部邮箱。

 

1.安装msmtp

 $ sudo apt-get install msmtp

 配置msmtp  
 在用户目录下创建文件,写入配置信息

 $ vim .msmtprc

> account default  
> host smtp.163.com    # SMTP邮件服务器地址  
> from sysmail@163.com    # 发送邮件的地址  
> auth plain  
> user sysmail@163.com    # 邮件服务器登陆账号  
> password xxxxxx    # 邮件服务器登录密码  
> logfile ~/.msmtp.log     # 日志位置 

2.安装mutt

  $ sudo apt-get install mutt

  配置mutt
  在用户目录下创建文件，写入配置信息

  $ vim .muttrc

> set realname="admin"  
> set from="sysmail@163.com"  
> set use_from=yes  
> set sendnmail="/usr/bin/msmtp"  

这就安装好了，测试一下

   $ mutt sendto -s "subject" -a attach < content

   sendto 是收件人邮箱地址  
   subjec 是主题  
   attach 是附件文件，以附件形式发送  
   content 是邮件正文  
