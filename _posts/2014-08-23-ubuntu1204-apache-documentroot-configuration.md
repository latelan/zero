---
layout: post
title: "ubuntu12.04 apache 更改默认目录"
description: ""
date: 2014-08-23
tags: []
---

基本配置  

>  系统:ubuntu12.04  
>  apache: apache 2.2.2

1.安装apache  

>  $sudo apt-get install apache2  

这样安装后默认网站目录是/var/www。我们需要更改这个目录。

2.更改默认目录  
修改/etc/apache2/sites-enabled/000-default.conf文件
 
> DocumentRoot /home/lat/web/  
>
> <Directory /home/lat/web>  
>	Options Indexes FollowSymLinks MultiViews  
>	AllowOverride None  
>	Order allow,deny  
>	allow from all  
> </Directory>  

3.重启apache  

>  $sudo /etc/init.d/apache2 restart

OK,现在网站根目录就在/home/lat/web下了。
