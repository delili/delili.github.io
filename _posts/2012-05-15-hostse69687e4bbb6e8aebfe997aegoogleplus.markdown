---
author: admin
comments: true
date: 2012-05-15 08:25:31+00:00
layout: post
slug: hosts%e6%96%87%e4%bb%b6%e8%ae%bf%e9%97%aegoogleplus
title: 修改hosts访问GooglePlus遇到404解决办法
wordpress_id: 364
categories:
- Google
- web
tags:
- Google
---

大概两周之前，google在大陆的ip好些有不能用了，听大牛说是google自己主动停用的，不知道为神马，于是好些人都是换了hosts或者开Gae才上来，hosts换了以后，偶尔会出现

[![google+404](http://i-deally.info/blog/wp-content/uploads/2012/05/Unnamed-QQ-Screenqwshot.bmp)](http://i-deally.info/blog/wp-content/uploads/2012/05/Unnamed-QQ-Screenqwshot.bmp)















这是hosts文件中plus.google.com对应ip有问题，大家一般都用的国内的ip，那段ip并不能提供google所有的服务，而且google的ip调整后Google+还会偶尔404，的确挺烦人，查了一下解决办法---换Google的国外ip

这就需要向国外的DNS查询，Dos下用nslookup命令，因为需要https协议，所以一般查询ip要确保443端口是开放的，一般都查accounts.google.com就可以了。

[![查询google的ip](http://i-deally.info/blog/wp-content/uploads/2012/05/QQ截图20120513205341.bmp)](http://i-deally.info/blog/wp-content/uploads/2012/05/QQ截图20120513205341.bmp)



















上图：向OpenDNS的服务器查询accounts.google.com得到IP

如果有司污染了所有的google.com域名并且封锁了just-ping网站怎么办呢？没关系，我们打开国内科技网站，搜索Google拿下某某域名的新闻，然后查询一下Google都拿下了那些域名，然后查询下那个域名就行了（大牛建议）

最后把对应ip写入hosts，刷新dns，重启浏览器即可。

附上搜到的ip号段，加入hosts，短期内应该不会出现什么问题：

    
    74.125.31.84 plus.google.com
    
    4.125.224.96 plus.google.com
    
    74.125.224.97 plus.google.com
    
    74.125.224.98 plus.google.com
    
    74.125.224.99 plus.google.com
    
    74.125.224.100 plus.google.com
    
    74.125.224.101 plus.google.com
    
    74.125.224.102 plus.google.com
    
    74.125.224.103 plus.google.com
    
    74.125.224.104 plus.google.com
    
    74.125.224.105 plus.google.com
    
    74.125.224.106 plus.google.com
    
    74.125.224.107 plus.google.com
    
    74.125.224.108 plus.google.com
    
    74.125.224.109 plus.google.com
    
    74.125.224.110 plus.google.com
    
    74.125.224.111 plus.google.com


PS:网友的力量是无穷的，你很少能有机会去第一个发现某个问题，遇到问题要盲目的去问别人，自己尝试着解决，有google在，你永远不是一个人^_^
参考：[http://mrzzm.blogspot.com/2012/03/hostsgoogle404.html](http://mrzzm.blogspot.com/2012/03/hostsgoogle404.html)






