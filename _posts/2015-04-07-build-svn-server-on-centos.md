---
layout: post
title: "在Centos上搭建SVN服务器"
description: ""
category: 
tags: [linux]
---
{% include JB/setup %}

闲来无事，在虚拟机中搭建一个代码托管服务，公司用的是svn，就拿svn做练习了。

1.安装SVN  

> $ sudo yum install subversion

2.新建版本库test

> $ sudo mkdir /home/svn  
> $ sudo svnadmin create /home/svn/test

3.将版本库test配置到Apache服务器（Http WebDAV方式）
安装dav模块

> $ sudo yum install mod_dav_svn

4.给SVN目录Apache访问的权限

> $ cd /home/svn  
> $ sudo chown -R apache.apache test  
> $ sudo chcon -R -t httpd_sys_content_t test

5.配置/etc/httpd/conf.d/subversion.conf文件

	<Location /svn>
	   DAV svn 
	   SVNParentPath /home/svn

	   AuthType Basic
	   AuthName "Authorization Realm"
	   AuthUserFile /etc/subversion/passwd
	   Require valid-user
	</Location>  

6.创建认证用户

> $ sudo htpasswd -c /etc/subversion/passwd latelan  

输入密码并确认,重启下httpd服务。

这样SVN服务器就搭建好了。使用浏览器访问http://localhost/svn/test，输入账号和密码即可浏览(当然，现在什么都没有)。如果有多个项目需要托管，执行步骤2和4。
