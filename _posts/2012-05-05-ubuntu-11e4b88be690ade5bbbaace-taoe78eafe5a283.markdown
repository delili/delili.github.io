---
author: admin
comments: true
date: 2012-05-05 14:39:57+00:00
layout: post
slug: ubuntu-11%e4%b8%8b%e6%90%ad%e5%bb%baace-tao%e7%8e%af%e5%a2%83
title: Ubuntu 11下搭建ACE/TAO环境
wordpress_id: 345
categories:
- linux
tags:
- ACE/TAO
---

1. 基本环境的下载安装

在终端运行 sudo apt-get install xxx 命令，其中xxx如下：

    
    
    build-essential   libssl-dev   libace-dev   libtao-dev   libtao-orbsvcs-dev   gperf   gperf-ace   tao-idl   mpc-ace
    




2. 环境变量配置

a. 打开/home/clark/目录下的.bashrc文件（注：clark为用户名，.bashrc文件为隐藏文件，可通过快捷方式ctrl+h显示），然后在其结尾添加如下内容：

    
    
    ##########################################
    
    # set ace/tao environment
    
    ACE_ROOT=/usr/lib/ace; export ACE_ROOT
    
    TAO_ROOT=$ACE_ROOT/TAO; export TAO_ROOT
    
    LD_LIBRARY_PATH=$ACE_ROOT/lib:$LD_LIBRARY_PATH; export LD_LIBRARY_PATH
    
    PATH=$ACE_ROOT/bin:$PATH; export PATH
    
    CIAO_ROOT=$TAO_ROOT/CIAO; export CIAO_ROOT
    
    DANCE_ROOT=$TAO_ROOT/DAnCE; export DANCE_ROOT
    
    ###########################################
    


然后在终端运行 source ~/.bashrc，使环境变量的设置生效

b. 修改 /usr/lib/ace/include/makeinclude/platform_marcos.GNU文件，在其结尾出添加如下内容（需要赋予该文件可写权限）：

    
    
    # for tao
    
    TAO_IDL := $(ACE_ROOT)/bin/tao_idl
    
    TAO_IDLFLAGS += -g $(ACE_ROOT)/bin/gperf
    
    TAO_IDL_DEP := $(ACE_ROOT)/bin/tao_idl$(EXEEXT)
    
    c. 添加tao_idl的快捷方式到bin下，在终端使用命令
    
    ln -s /usr/lib/ace/TAO/tao_idl /usr/lib/ace/bin/tao_idl
    




3. 安装QT，（项目界面用QT实现）完成之后安装qt-ace/tao库，运行命令 sudo apt-get install libtao-qtresource-dev。到此环境配置结束。



4. 在新建Qt GUI项目时，需向其**.pro文件中添加如下内容：

    
    
    INCLUDEPATH += /usr/include/ace /usr/include/tao
    
    LIBS += -L/usr/lib -lACE -lTAO -lTAO_PortableServer -lTAO_AnyTypeCode -lTAO_QtResource.
    


注： 以上是在ubuntu  11 下的搭建，10及几下版本的环境跟11有很大区别，很多配置不一样，可能会发生找不到软件包或者缺少配置文件的错误，大家在配置的时候需要注意。
