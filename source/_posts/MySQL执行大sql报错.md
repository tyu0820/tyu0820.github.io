---
title: MySQL执行大sql报错
categories: MySQL
tags: MySQL
---

“Packet for query is too large (1706 > 1024). You can change this value on the server by setting the max_allowed_packet' variable. ”

这个错误可能是MySQL max_allowed_packet默认值太小导致的，可以修改my.ini中的配置解决。

查看目前配置：

	show VARIABLES like '%max_allowed_packet%';

在配置文件中找到**[mysqld]**，在这个位置下面修改max_allowed_packet配置即可。

	[mysqld]
	datadir=/var/lib/mysql
	socket=/var/lib/mysql/mysql.sock
	user=mysql
	#Disabling symbolic-links is recommended to prevent assorted security risks
	symbolic-links=0
	max_allowed_packet=20M


## linux下查看mysql配置文件

### 方法一

通过以下命令即可查询配置文件位置： 

	mysql --help | grep my.cnf

### 方法二

1、通过which命令查看mysql在哪

	which mysql

显示出目录例如：

	/usr/bin/mysql

2、查询配置文件在哪

	/usr/bin/mysql --verbose --help | grep -A 1 'Default options'

执行该命令后显示出如下信息：

	Default options are read from the following files in the given order:
	/etc/mysql/my.cnf /etc/my.cnf ~/.my.cnf 

服务器首先读取的是/etc/mysql/my.cnf文件，如果前一个文件不存在则继续读/etc/my.cnf文件，如若还不存在便会去读~/.my.cnf文件

## vim编辑配置文件

	vim /etc/my.cnf

	:w 保存文件但不退出vi.
	:w file 将修改另外保存到file中，不退出vi.
	:w! 强制保存，不推出vi.
	:wq 保存文件并退出vi.
	:wq! 强制保存文件，并退出vi.
	q: 不保存文件，退出vi.
	:q! 不保存文件，强制退出vi.
	:e! 放弃所有修改，从上次保存文件开始再编辑
