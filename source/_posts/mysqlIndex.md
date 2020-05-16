---
title: mysql索引优化
date: 2019-04-02 18:27:44
tags: 优化
categories: 
- 数据库
- mysql
---
## 1 查看查询语句执行效率
**语法**
explain select … from … [where ...]
**示例**
```
explain select * from news;
```
![explain](mysqlIndex/exp.png)

只查询一行数据：
![const](mysqlIndex/const.png)

查询多行数据：执行查询
![towrow](mysqlIndex/towrow.png)
执行explain:
![towrowres](mysqlIndex/towrowres.png)

## 2 属性详解
| 属性 | 作用 | 
| --- | --- |
| id | SELECT的查询序列号 |
| select_type | SELECT的查询序列号 |
| table | 显示这一行的数据是关于哪张表的 |
| type | 显示了连接使用了哪种类别,有无使用索引，是使用Explain命令分析性能瓶颈的关键项之一 |
| possible_keys | 指出MySQL能使用哪个索引在该表中找到行 |
| key | 显示MySQL实际决定使用的键（索引）。如果没有选择索引，键是NULL |
| key_len | 显示MySQL决定使用的键长度。如果键是NULL，则长度为NULL。使用的索引的长度。在不损失精确性的情况下，长度越短越好 |
| ref | 显示使用哪个列或常数与key一起从表中选择行 |
| rows | 显示MySQL认为它执行查询时必须检查的行数 |
| Extra | 包含MySQL解决查询的详细信息，也是关键参考项之一 |
**select_type**

| 类型 | 解释 | 
| --- | --- |
| SIMPLE | 简单SELECT(不使用UNION或子查询等) |
| PRIMARY | 最外面的SELECT |
| UNION | UNION中的第二个或后面的SELECT语句 |
| DEPENDENT UNION | UNION中的第二个或后面的SELECT语句，取决于外面的查询 |
| UNION RESULT | UNION的结果 |
| SUBQUERY | 子查询中的第一个SELECT |
| DEPENDENT SUBQUERY | 子查询中的第一个SELECT，取决于外面的查询 |
| DERIVED | 导出表的SELECT(FROM子句的子查询) |
**type**

索引从好到坏依次是：
```
system > const > eq_ref > ref > fulltext > ref_or_null > index_merge > unique_subquery > index_subquery > range > index > ALL
```
一般来说，得保证查询至少达到range级别，最好能达到ref，否则就可能会出现性能问题。
