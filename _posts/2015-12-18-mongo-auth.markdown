---
author: deli
comments: true
date: 2015-12-18 16:04:52+00:00
layout: post
title: mongodb访问控制
categories:
- 技术
tags:
- 数据库 mongodb  权限
---


2年前用过一次mongodb，当时没有注意到它的权限方面的问题，这次作为简单的存储又用到了，且开启权限控制的花了半天时间...在这里记录一下也防止别人再踩坑吧。

首先要说明的是，mongodb默认是不开启权限认证的，即默认登陆没有用户名和密码，这也是我之前使用的时候没有关注它权限的原因。最后设置成功主要参考了[这篇博客][1]，这也是第一次感到stackoverflow的无力。

-  坑1 [MongoDB-CR Authentication failed][2]

用户在客户端中认证通过，即 authenticate user from mongo it returns 1,但登录时仍显示失败并这个错误，这时候需要修改认证机制(authentication mechanism)，并重新登录，对应的操作为：

```
mongo
use admin
db.system.users.remove({})    //removing all users
db.system.version.remove({}) //removing current version 
db.system.version.insert({ "_id" : "authSchema", "currentVersion" : 3 })
```

原因是mongodb升级到3.0之后也升级了[验证策略][3]，而我们建立的用户仍然用的是MONGODB-CR这个策略，所以需要我们将验证策略改回3，删除之前的所有用户再新建用户。

- 坑2  Property 'addUser' of object test is not a function

同是版本问题，升级到3.0之后，addUser函数改为createUser()，如果我们参考的是之前的版本来建立用户的话，则会报错。3.0之后建立用户须传递的对象如下，详细参数可参考[官方文档][4]：

```
{
	user: "username",
	pwd: "passphrase",
  	roles: [{ 
        role: "userAdminAnyDatabase", 
        db: "admin" 
    }］
}
```

- 坑3 配置文件中设置mongodb的用户名和密码项

```
<bean id="mongoDbFactory" class="org.springframework.data.mongodb.core.SimpleMongoDbFactory">
    <constructor-arg name="mongo" ref="mongo" />
    <constructor-arg name="databaseName" value="databaseName" />
    <constructor-arg name="credentials" ref="userCredentials" />
</bean>
<bean id="userCredentials" class="org.springframework.data.authentication.UserCredentials">
    <constructor-arg name="username" value="username" />
	<constructor-arg name="password" value="password" />
</bean>
```

其余就是一步步的配置了，参考[这篇博客][5]就好。

博客荒废了好久，希望最近跑实验有问题能够记录在此。


[1]: http://ibruce.info/2015/03/03/mongodb3-auth/
[2]: http://stackoverflow.com/questions/29006887/mongodb-cr-authentication-failed
[3]: https://docs.mongodb.org/master/release-notes/3.0-scram/#upgrade-scram-scenarios
[4]: https://docs.mongodb.org/manual/reference/method/db.createUser/
[5]: http://ibruce.info/2015/03/03/mongodb3-auth/

