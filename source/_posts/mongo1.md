---
title: mongodb使用
date: 2019-04-02 18:27:44
categories: 
- 数据库
- mongodb
---
# 基础
* 列出所有数据库:show dbs;
* 列出所有表：show tables;或show collections;
* 查询所有数据：db.taname.find().pretty();
* 查询指定列所有数据：db.tbname.find({},{col1:1,col2:2,...}).pretty();
* 根据条件查询所有数据：db.tbname.find({name:"test"},{col1:1,col2:1}).pretty();
* 查询结果不包含指定字段：db.tbname.find({},{status:0,col2:0}).pretty();
# 查询
#### 1 查询所有数据
db.tbname.find().pretty();
