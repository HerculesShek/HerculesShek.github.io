---
layout: post
title: MySql远程客户端连接不上的问题
category: blog
description: mysql客户端有时连不上远程的数据库，大部分是默认权限的问题
---

在你有远程服务器访问权限的时候，如果首次登陆不上，估计是服务器端的设置出了问题
解决方法有两个：
## 一、修改host
在服务器上登陆进入MySql，更改mysql数据库（这个看不见的）中的user表，修改host列就行了：

	use mysql
	
之后看一下user表：

	select * form user
	
再修改表

	update user set host = '%' where user = 'root';
	
## 二、增加用户：

	GRANT ALL PRIVILEGES ON *.* TO 'username'@'192.168.0.104' IDENTIFIED BY 'pass' WITH GRANT OPTION;
	
允许用户名为username的用户从192.168.2.104的主机以密码为pass登陆


	GRANT ALL PRIVILEGES ON *.* TO 'username'@'%' IDENTIFIED BY 'pass' WITH GRANT OPTION;

允许用户名为username的用户从任意主机以密码为pass登陆

