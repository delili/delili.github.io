---
author: admin
comments: true
date: 2012-05-14 17:50:03+00:00
layout: post
slug: ubuntu%e4%b8%8bjava%e7%8e%af%e5%a2%83%e9%85%8d%e7%bd%ae
title: ubuntu下java环境配置
wordpress_id: 356
categories:
- linux
tags:
- java
- linux
- source
---

（第一段不是整体，纯属自己墨迹，不喜欢看的可以跳过）

以前用ubuntu10的时候印象里没有用到过java，当时唯一的记忆就是装过python，因为自己比较喜欢折腾，所以什么东西都想整整，python自己过两个星期吧，没写过什么小东西，计划这个暑假再好好学学（这个计划一定得付诸实施），继续，两个月之前迫于某些原因，从10升到11了，说实话，开始不习惯11的界面，后来偏偏却爱上这个设计了，Unity，自己用着特别舒服，挺佩服设计师们的创意的，好像12.04又升级了，增强了Unity的隐私功能，挺好。

下面是具体的环境配置：

1.安装jdk

2种方式：

终端安装

离线下载

我直接下载解压安装的，东西太大终端安怕会意外中断，其实多半是我多操心，linux的稳定性就不用我说了，我主要是担心手头这个长宽。

网上下载安装包大概有3中，rpm，bin，gar.tz

java会自动识别操作系统，ubuntu下载bin就可以直接在终端下安装了，rpm的安装可以参考

[http://blog.csdn.net/neohuo/article/details/600339](http://blog.csdn.net/neohuo/article/details/600339)

都很直接，解压也是同样的道理。

2.然后就是环境环境变量的配置

参考

http://blog.donews.com/javapro/archive/2005/10/07/579679.aspx（说的挺好的）

1） PATH环境变量。作用是指定命令搜索路径，在shell下面执行命令时，它会到PATH变量所指定的路径中查找看是否能找到相应的命令程序。我们需要把jdk安装目录下的bin目录增加到现有的PATH变量中，bin目录中包含经常要用到的可执行文件如javac/java/javadoc等待，设置好PATH变量后，就可以在任何目录下执行javac/java等工具了。

2）

CLASSPATH环境变量。作用是指定类搜索路径，要使用已经编写好的类，前提当然是能够找到它们了，JVM就是通过CLASSPTH来寻找类的。我们需要把jdk安装目录下的lib子目录中的dt.jar和tools.jar设置到CLASSPATH中，当然，当前目录“.”也必须加入到该变量中。

3）

JAVA_HOME环境变量。它指向jdk的安装目录，Eclipse/NetBeans/Tomcat等软件就是通过搜索JAVA_HOME变量来找到并使用安装好的jdk。



三种配置环境变量的方法

1） 修改/etc/profile文件

如果你的计算机仅仅作为开发使用时推荐使用这种方法，因为所有用户的shell都有权使用这些环境变量，可能会给系统带来安全性问题。

·用文本编辑器打开/etc/profile

·在profile文件末尾加入：

    
    
    JAVA_HOME=/usr/share/jdk1.5.0_05
    
    PATH=$JAVA_HOME/bin:$PATH
    
    CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
    
    export JAVA_HOME
    
    export PATH
    
    export CLASSPATH
    


·重新登录

·注解

a. 你要将 /usr/share/jdk1.5.0_05jdk 改为你的jdk安装目录

b. linux下用冒号“:”来分隔路径

c. $PATH / $CLASSPATH / $JAVA_HOME 是用来引用原来的环境变量的值在设置环境变量时特别要注意不能把原来的值给覆盖掉了，这是一种

常见的错误。

d.CLASSPATH中当前目录“.”不能丢,把当前目录丢掉也是常见的错误。

e. export是把这三个变量导出为全局变量。

f. 大小写必须严格区分。

2. 修改.bashrc文件

这种方法更为安全，它可以把使用这些环境变量的权限控制到用户级别，如果你需要给某个用户权限使用这些环境变量，你只需要修改其个人用户主目录下的.bashrc文件就可以了。

·用文本编辑器打开用户目录下的.bashrc文件

·在.bashrc文件末尾加入：

    
    
    set JAVA_HOME=/usr/share/jdk1.5.0_05
    
    export JAVA_HOME
    
    set PATH=$JAVA_HOME/bin:$PATH
    
    export PATH
    
    set CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
    
    export CLASSPATH
    


·重新登录

3. 直接在shell下设置变量

不赞成使用这种方法，因为换个shell，你的设置就无效了，因此这种方法仅仅是临时使用，以后要使用的时候又要重新设置，比较麻烦。

只需在shell终端执行下列命令：

    
    
    export JAVA_HOME=/usr/share/jdk1.5.0_05
    
    export PATH=$JAVA_HOME/bin:$PATH
    
    export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
    


##############################

此处介绍一个命令

source

source可以强行让一个脚本去影响当前的环境(他执行该脚本中的全部命令,而不关脚本文件的权限如何设置).

source命令(从 C Shell 而来)是bash shell的内置命令。点命令，就是一个点符号，(从Bourne Shell而来)是source的另一名称。

同样的，当前脚本中设置的变量也将作为脚本的环境，source(或点)命令通常用于重新执行刚修改的初始化文件，如 .bash_profile 和 .profile 等等。

例如，如果在登录后对 .bash_profile 中的 EDITER 和 TERM 变量做了修改，则可以用source命令重新执行 .bash_profile 中的命令而不用注销并重新登录。把两个命令用&&联接起来，如 make mrproper &&make menuconfig ，表示要第一个命令执行成功才能执行第二个命令。



参考：[http://www.linuxsense.org/archives/137.html](http://www.linuxsense.org/archives/137.html)

这里讲的也不错[ http://blog.csdn.net/simon_dong618/article/details/1581132]( http://blog.csdn.net/simon_dong618/article/details/1581132)

懂了吧，修改环境变量，不用重新登录喽，还不赖吧，有兴趣的话自己去探索探索，我在这里就不赘述了。

java的配置就到这里，尽情的在java的世界挥霍吧
