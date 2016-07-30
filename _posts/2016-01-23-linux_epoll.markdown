---
author: deli
comments: true
date: 2016-01-23 22:04:52+00:00
layout: post
title: linux内核中的epoll
categories:
- 技术
tags:
- linux epoll Nginx
---


最近学习中接触到了Nginx，虽然只用于本地转发请求，但还是对其实现中的epoll进行了较为详细的学习，遂在此记录。

参考讨辉老师的《深入理解Nginx》，以Linux内核2.6.35版本为例，简单记录下epoll的内部主要数据结构是如何安排的。

###   1.epoll中的系统调用    
epoll 的接口如下,主要是 epoll_create，epoll_ctl 和 epoll_wait 三个函数:

#####   int epoll_create(int size);
创建一个epoll的句柄，size用来告诉内核这个监听的数目一共有多大。

#####   int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event);
向epoll对象中添加、修改或者删除事件。


#####  int epoll_wait(int epfd, struct epoll_event * events, int maxevents, int timeout);
收集epoll监控的事件中已经发生的事件，如果没有任何事件发生，则等待timeout毫秒返回。


###   2.epoll中的主要数据结构

>
	 struct eventpoll {
	 	...
	 	struct list_head rdllist;    //事件满足条件的链表
	 	struct rb_root rbr;          //用于管理所有fd的红黑树
	 	...
	 }


每个epoll对象都有一个独立的eventpoll结构体，该结构体会在内核空间中创建独立的内存，用来存储epoll_ctl方法向epoll对象中添加进来的事件。事件都会挂到红黑树rbr下，利用红黑树可以实现事件的快速添加和删除。


>
	struct epitem {
		...
		struct rb_node rbn;            //用于主结构管理的红黑树
		struct list_head rdllink;      //事件就绪队列
		struct epoll_filefd ffd;       //每个fd生成的一个结构
		struct eventpoll *ep;          //该项属于哪个主结构体
		struct epoll_event event;  		//注册的感兴趣的事件,也就是用户空间的epoll_event
		...
	}


每个事件都会建立一个epitem结构体；并与设备驱动程序建立回调关系，相应的事件发生时会调用这里的回调方法（ep_epoll_callback）,它会把这样的事件放到上面的rllist双向链表中；

当调用epoll_wait检查是否有事件发生时，只需要检查eventpoll对象中的rdlist双向链表是否有epitem，如果rdlist链表不为空，则把这里的事件复制到用户内存中。


在查找资料的过程中，了解到了很久之前的C10K问题，以及现在的C100K，C1M，C10M，C100M...等问题，不得不说，年轻人还是得多学习，提升自己的level！


参考:
[epoll的优点及epoll学习心得][1]
[深入理解Nginx][2]

[1]: http://www.cppblog.com/flashboy/archive/2008/04/16/47277.html
[2]: https://book.douban.com/subject/22793675/
