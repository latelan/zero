---
layout: post
title: "study epoll"
description: ""
date: 2014-08-22
tags: [epoll]
---

epoll系列系统调用

epoll使用一组函数来完成任务，epoll把用户关心的文件描述符上的事件放在内核里的一个事件表中，从而无须像select和poll那样每次调用都要重复传入文件描述符集或事件集。但epoll需要使用一个额外的文件描述符，来唯一标识内核中的这个时间表。这个文件描述符使用如下epoll_create函数来创建：

```c
#inculde <sys/epoll.h>
int epoll_create(int size);
```

size参数现在并不起作用，只是给内核一个提示，告诉它事件表需要多大。该函数返回的文件描述符将作用其他所有epoll系统调用的第一个参数，以指定要访问的内核事件表。

下面的函数用来操作epoll的内核事件表：  

```c
#include <sys/epoll.h>
int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event)
```
fd参数是要操作的文件描述符，op参数则指定操作类型。操作类型有如下3种：  

> EPOLL_CTL_ADD 往事件表种注册fd上的事件。  
> EPOLL_CTL_MOD 修改fd上的注册事件。  
> EPOLL_CTL_DEL 删除fd上的注册事件。  

event参数指定事件，它是epoll_event结构指针类型。epoll_event的定义如下：

```c
struct epoll_event
{
	__uint32_t event;	/* epoll事件*/
	epoll_data_t data;	/* 用户数据 */
};
```
epoll_wait函数

epoll系列系统调用的主要接口是epoll_wait函数。它在一段超时时间内等待一组文件描述符上的事件。

```c
#include <sys/epoll.h>
int epoll_wait(int epfd, struct epoll_event *events, int maxevents, int timeout );
```

该函数成功时返回就绪的文件描述符的个数，失败时赶回-1并设置errno。  
timeout参数指定epoll的超时值，单位是毫秒。当timeout为-1时，epoll调用将永远阻塞，知道某个事件发生；当timeout为0时，epoll调用将立即返回。  
maxevents参数指定最多监听多少个事件，它必须大于0。  
epoll_wait函数如果检测到事情，就将所有就绪的事件从内核事件表（由epfd参数指定）中复制到它的第二个参数events指向的数组中。这个数组只用于输出epoll_wait检测到的就绪事件，而不像select和poll的数组参数那样既用于传入用户注册的事件，有用于输出内核检测到的就绪事件。这极大地提高了应用程序索引就绪文件描述符的效率。

LT和ET模式

epoll对文件描述符的操作有两种模式：LT（Level Trigger, 电平触发）模式和ET（Edge Trigger， 边沿触发）模式。LT模式是默认的工作模式，这种模式下epoll相当于一个小路较高的poll。当往epoll内核事件表中注册一个文件描述符上的EPOLLET事件时，epoll将以ET模式来操作该文件描述符。ET模式是epoll的高校工作模式。

对于采用LT工作模式的文件描述符，当epoll_wait检测到其上有事件发生并将此事件通知应用程序后，应用程序可以不立即处理该事件。这样，当应用程序下一次调用epoll_wait时，epoll_wait还会再次向应用程序通告事件，知道该事件被处理。而对于采用ET工作模式的文件描述符，当epoll_wait检测到其上有事件发生并将此事件通知应用程序后，应用程序必须立即处理该事件，因为后续的epoll_wait调用将不在向应用程序通知这一事件。ET模式在很大程度上降低了同一个epoll事件被重复触发的次数，因此效率要比LT模式高。
