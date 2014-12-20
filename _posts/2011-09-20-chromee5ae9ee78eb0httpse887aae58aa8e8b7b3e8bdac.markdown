---
author: admin
comments: true
date: 2011-09-20 15:16:21+00:00
layout: post
slug: chrome%e5%ae%9e%e7%8e%b0https%e8%87%aa%e5%8a%a8%e8%b7%b3%e8%bd%ac
title: chrome实现HTTPS自动跳转
wordpress_id: 221
categories:
- web
tags:
- https
- 重定向
---

前阵子一直在纠结网站的HTTPS 不能自动跳转的问题,比如facebook,twitter...等,因为笔者现在用的是HOSTS 访问,所以会出现访问上去的时候,但是将通信协议换成HTTPS就可以访问了,原理我就不用说了吧,每次都自己去地址栏那加,总觉得很麻烦,找了半天插件也没有找到...自己水平有限,也不会写,有哪位神牛能写一下最好了....



步入正题:

在网站找了一下相关了资料,发现chrome 自身还是很强大的,自己就实现了这个功能(又让我刮目相看了)...下面说一下具体配置方法:



	
  1. 地址栏输入chrome://net-internals/

	
  2. 在HSTS的标签栏里Domain里填上需要访问的domain,(我尝试了google.com,但是好像没实现自动跳转,还请大牛指点),twitter,facebook 都是可以的

	
  3. 然后选中Include subdomains点击Add按钮即可，可以加多个域

	
  4. 这样所有访问这个域名（包括子域名）都自动转到https了


还是很简单的吧....

(我所谓的自动跳转,貌似专业术语叫做重定向,这里就不更正了,以后的文章会学着专业点的^-^)
