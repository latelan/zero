---
layout: post
title: "How to set lock screen wallpaper with EarthLiveSharp"
description: "设置锁屏壁纸为地球照片并定时修改"
date: 2017-05-24
tags: []
---

在github上看到[EarthLiveSharp](https://github.com/bitdust/EarthLiveSharp)，感觉挺有意思，拉取最新的himawari8观测的地球照片当做桌面。于是就下来使用了，的确不错。默认每隔20分钟会更新一次桌面。用了一段时间后发现一个问题，大部分时间都是开着各种软件、编辑器独占屏幕，很少有时间把桌面亮出来看，更多时候离开座位就锁屏了。于是乎想着能不能让其自动修改锁屏背景呢。

目标：定时修改锁屏壁纸为最新的地球照片。

原来在win7下修改锁屏壁纸不是一件容易的事情。修改注册表，修改管理策略，还要把图片放在指定位置，指定名称，大小还有限制。不过还是有人找到了方法，参考这篇文章[《win7设置锁屏背景壁纸并自动定时修改》](http://www.capjsj.cn/lock_screen_background.html)可以做到修改锁屏壁纸以及使用任务计划功能实现定期修改。但图片不能简单的从EarthLiveSharp中拷贝，桌面背景的设置是调用API，API提供了放置图片的方式。设置合适的方式，一个550\*550的图片可以放到1920\*1080的屏幕上也可以放到1280*1024的屏幕上而不会变形。但锁屏壁纸需要的是一张格式为jpg的图片，没有API可用，所以我们需要做出来相应的这张照片，然后拷贝到相应的位置。

修改了下EarthLiveSharp的代码，将制作过程加了进去，代码见[这里](https://github.com/latelan/EarthLiveSharp/blob/master/EarthLiveSharp/Program.cs#L180-L210)。这样每次拉到新图片时会做一张适合当前屏幕分辨率的锁屏壁纸（backgroundDefault.jpg）在```images```目录中。

这样图片的问题解决，接下来让其定期更新。做一个bat文件，名字叫做```change_lock_screen_wallpaper.bat```。
```bat
@echo off
set imgPath="E:\EarthLiveSharp\images\backgroundDefault.jpg"
cd "C:\Windows\System32\oobe\info\backgrounds"
copy %imgPath% backgroundDefault.jpg
```
在开始菜单中搜索```任务计划```，添加一个文件夹，再创建一个任务，在常规中选择```使用最高权限运行```。触发器设置为每隔15分钟执行一次，大致能匹配上EarthLiveSharp的更新频率。操作选择```启动程序```，路径填写```change_lock_screen_wallpaper.bat```的路径。添加后立即执行下看是否成功。

![background-default](/assets/img/background-default.jpg)

