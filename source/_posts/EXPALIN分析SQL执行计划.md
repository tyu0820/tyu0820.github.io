---
title: EXPALIN分析SQL执行计划
categories: MySQL
tags: MySQL
---

## EXPLAIN基本语法

	EXPLAIN SELECT * FROM test ORDER BY scrq;

## 字段说明

### id
 
标识select查询的序列号。如果没有子查询或者关联查询，只有唯一的select，每行都将显示1；如果有子查询或者关联查询内层的select语句一般会顺序编号。

- id相同：从上到下顺序执行
- id不同：值越大，优先级越高，越先被执行
- id为null：表示用来合并结果集的，在sql中使用union时就会出现

### select_type 
- simple：简单查询，不包含子查询和union
- primary：包含union或者子查询，最外层的部分标记为primary
- subqery：一般子查询中的子查询被标记为subquery，也就是位于select列表中的查询
- derived：派生表-该临时表是从子查询派生出来的，位于form中的子查询
- union：位于union中第二个及其以后的子查询被标记为union，第一个就被标记为primary，如果是union位于from中则标记为derived
- union result：用来从匿名临时表里检索结果的select被标记为union result
- dependent union：首先需要满足UNION的条件，及UNION中第二个以及后面的SELECT语句，同时该语句依赖外部的查询
- subquery：子查询中第一个SELECT语句
- dependent subquery：和DEPENDENT UNION相对UNION一样

### table 
访问查询的表名或表别名

### type
表的访问类型，由上至下，效率越来越高

- ALL：全表扫描，一般这样的出现这样的SQL而且数据量比较大的话那么就需要进行优化了，要么是这条SQL没有用上索引，要么是没有建立合适的索引。
- index：全索引扫描，这个比all效率要好一点；如果Extra中Using Index与Using Where同时出现的话，则是利用索引查找键值的意思。
- range：索引范围扫描，常用于<, <=, >=, between等操作。
- index_subquery：在 某 些 IN 查 询 中 使 用 此 种 类 型 , 与 unique_subquery 类似,但是查询的是非唯一 性索引。
- unique_subquery：在某些 IN 查询中使用此种类型,而不是常规的 ref。
- index_merge ： 说明索引合并优化被使用了。
- ref_or_null ： 如同 ref, 但是 MySQL 必须在初次查找的结果 里找出 null 条目,然后进行二次查找。
- ref: 使用了非唯一性索引进行数据的查找，例如：我们对用户表的用户名这一列建立了非唯一索引，因为用户名可以重复，当我们查找用户的时候select * from user where username=“xxx”的时候就出现了ref，使用非唯一索引查找数据。
- eq_ref:类似ref，区别在于使用的是唯一索引，使用主键的关联查询
- const:通常情况下，将一个主键放置到where后面作为条件查询，mysql优化器就能把这次查询优化转化为一个常量，如何转化以及何时转化，这个取决于优化器。这个比eq_ref效率高一点。
- system：表只有一行。
- null：MySQL不访问任何表或索引，直接返回结果
 
### possible_keys
这个显示可能用到的索引，一个列上可能有多个复合索引，但最后只能选择其中一个使用。当然很可能一个都不会使用，也可能显示没有可能用上的索引，但最后却用上了某个索引。

### key
真实的反应了MySQL使用了哪个索引。这个字段很关键，因为MySQL优化器的能力有限，有时候他使用的索引不一定是最优的，所以我们需要知道他到底使用了那个索引，以及强制MySQL使用某个DBA认为最优的索引。当然如果当前的查询是覆盖索引的话，这个索引也会出现在key中。

### key_len
表示索引中使用的字节数，可通过该key_len计算查询中使用的索引长度，当然在不损失精度的情况下长度越短越好。但是这个key_len只是表示使用索引的长度最大可能是多少，并不是真实的长度。

### ref
显示索引的哪一列被使用了，如果可能的话，是一个常数。

### rows
根据表的统计信息及索引使用情况，大致估算出找出所需的记录需要读取的行数。这个rows非常重要，直接反应的SQL找了多少数据。在完成目的的情况下当然是越少越好。这个更SQL使用的索引息息相关。

### extra
- using filesort：说明mysql无法利用索引进行排序，那他只能用排序算法重新进行排序了，这会额外消耗资源。出现这个的话那么说明这条SQL需要优化了，需要正确使用索引或者建立合适的索引。
- using temporary：建立了临时表来保存中间结果，查询完成之后又要把临时表删除。出现这样的情况说明这条SQL语句请一定要优化，如果这样的SQL一多的话非常影响系统的性能。
- using index：这个表示当前的查询是覆盖索引的，直接从索引中读取数据，而不用访问数据表。如果同时出现using where表明索引被用来执行索引键值的查找，如果没有using where，表明索引被用来读取数据，而不是进行查找。
- using where：使用了where进行条件过滤。
- using join buffer：表示使用了连接缓存。
- impossible where：where语句的值总是false。
- Distinct：一旦MySQL找到了与行相联合匹配的行，就不再搜索了。