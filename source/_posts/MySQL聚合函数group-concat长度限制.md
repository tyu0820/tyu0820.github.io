---
title: MySQL聚合函数group-concat长度限制
categories: MySQL
tags: MySQL
---

GROUP_CONCAT 特别是数据比较多的时候 可能会截断 （GROUP_CONCAT里 数据量大的尽量不要使用）。

在MySQL配置文件(my.ini)中加上如下配置并重启mysql(-1为最大值或填入你要的最大长度)

	group_concat_max_len = -1 

在客户端执行以下语句：

	show variables like "group_concat_max_len";  

如果为自己修改的值或4294967295（设置为-1时）则修改正确。