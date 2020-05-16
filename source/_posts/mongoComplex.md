---
title: mongodb学习记录
date: 2019-10-20 18:27:44
categories: 
- 数据库
- mongodb
---
# 参考文档
http://www.mongoing.com/docs/reference/operator/aggregation/interface.html    
https://www.cnblogs.com/zhoujie/p/mongo1.html    
https://www.docs4dev.com/docs/zh/mongodb/v3.6/reference/tutorial-text-search-in-aggregation.html
# mongodb精准匹配
假设有需求如下：数据为多层级的数组，需要精准匹配到某一个层级，并进行更新或新增等操作，如何实现？    
## 实现方式
当Mongodb版本大于3.6.1，实现将会非常简单，因为mongodb3.6.1以上可以直接通过语句来实现精准匹配。    
但由于实际场景中版本不支持，当匹配超过一级则会报错：Too many positional (i.e. '$') elements found in path 'files.$.testConstruct.params.list.$.testRange'        
当版本大于3.6.1，举例：    
```
db.test_db.update(
    {'_id': {$in: ['1242rererwwr']}, 'files.name': 'test1.json', 'files.testConstruct.params.list.type': 'NewData'},
    {$set: {'files.$.structuredContent.params.list.$.testRange': {'enable': false, 'min': 14, 'max': 90}}, $currentDate: { updatedDatetime: true }}, { multi: true })
```
分析：    
设置值时，'files.$.testConstruct.params.list.$.testRange'中的第一个"$"将会匹配到name=test1.json的数组，第二个"$"将会匹配到files.structuredContent.params.list.type=NewData的数组    
优点： 使用简单，匹配精确，无需代码判断    
缺点： 适用版本有限制，且由于多级匹配需要更多的条件，而实际业务中不一定能取到这些条件（例如知道要更新的key为testRange但是不知道上级查询条件无法匹配到）    
如果版本过低，可只匹配第一级，其余的通过代码匹配。    
当版本小于3.6.1举例：
```php
<?php
public function batchUpdateContents(string $route, array $ids, string $editor, $updateItems, bool $needPublish): bool
    {
        $routes = explode('-', $route);
        array_shift($routes);
        $routes[2] = self::TYPE_LIST[$routes[2]];
        $updateRoute = 'files.$.testConstruct.' . implode('.', $routes);
        $res = $this->conn->update(['storeId' => ['$in' => $ids], 'files.name' => 'makeup.json', 'files.testConstruct' => ['$ne' => '']],
            ['$set' => [$updateRoute => $updateItems, 'files.$.editor' => $editor, 'files.$.lastUpdate' => time()], '$currentDate' => [ 'updatedDatetime' =>  true ]], true);
        if ($res) {
            // 一些其他操作
        } else {
            return false;
        }
    }
?>
```
枚举出可能存在的下一级数组值，然后进行匹配。update()方法当参数不存在时会进行新增参数操作。    
# mongodb聚合
mongodb的聚合操作可对数据进行一系列操作并返回结果。mongodb提供三种聚合操作：    
aggregation pipeline, map-reduce方法和分片集合
## aggregation pipeline
管道操作，基础操作为筛选操作，其他操作：可通过特定的字段来进行分组和排序等
### $project
类似于sql的select，筛选出需要查询的字段，例如：{$project: {'name': 1, 'age':1}},也可以排除字段（0）,重命名字段，派生字段    
（1）<field>: <1 or true>    是否包含该字段，field:1/0，表示选择/不选择 field    
（2）<field>:<0 or false>    v3.4新增功能，指定排除字段     
（3）<field>: <expression>  添加新字段或重置现有字段的值。 在版本3.6中更改：MongoDB 3.6添加变量REMOVE。如果表达式的计算结果为$$REMOVE，则该字段将排除在输出中。
3.6可使用变量REMOVE来有条件地禁止一个字段：    
```  
db.books.aggregate( [
   {
      $project: {
         title: 1,
         "author.first": 1,
         "author.last" : 1,
         "author.middle": {
            $cond: {
               if: { $eq: [ "", "$author.middle" ] },
               then: "$$REMOVE",
               else: "$author.middle"
            }
         }
      }
   }
] )

```
#### 使用$project派生举例    
```  
db.books.aggregate(
   [
      {
         $project: {
            title: 1,
            isbn: {
               prefix: { $substr: [ "$isbn", 0, 3 ] },
               group: { $substr: [ "$isbn", 3, 2 ] },
               publisher: { $substr: [ "$isbn", 5, 4 ] },
               title: { $substr: [ "$isbn", 9, 3 ] },
               checkDigit: { $substr: [ "$isbn", 12, 1] }
            },
            lastName: "$author.last",
            copiesSold: "$copies"
         }
      }
   ]
)
```
#### 投影出新数组：    
示例数据：
```  
{ "_id" : ObjectId("55ad167f320c6be244eb3b95"), "x" : 1, "y" : 1 }
```
操作：
``` 
db.collection.aggregate( [ { $project: { myArray: [ "$x", "$y" ] } } ] )
```
返回：
``` 
{ "_id" : ObjectId("55ad167f320c6be244eb3b95"), "myArray" : [ 1, 1 ] }
```
如果返回的数组中包含了不存在的字段，则会返回null
### $match
类似于sql中的where，设置查询条件，例如：{$match: {'name': {'$ne': ''}}}    
$match仅接受一个指定查询条件的文档，查询语法与读操作查询语法相同。    
#### 语法
{$match: {<query>}}    

#### 注意事项
在实际应用中尽可能将$match放在管道的前面位置。这样有两个好处：一是可以快速将不需要的文档过滤掉，以减少管道的工作量；二是如果再投射和分组之前执行$match，查询可以使用索引。    
#### 限制条件   
（1）不能在$match查询中使用$作为聚合管道的一部分    
（2）要在$match阶段使用$text，$match阶段必须是管道的第一阶段
#### 查询条件介绍
$gt:大于
$lt:小于    
$gte:大于等于     
$lte:小于等于    
$in:类似于sql中的in    
$nin:不在该范围内的键    
$or:包含多个可能的条件
### $limit
限制传递到管道中下一阶段的文档数
### skip
跳过指定数量的文档，并将其余文档传递到管道中的下一阶段
### $unwind
从输入文档解构数组字段以输出每个元素的文档，即：将数组拆分成单独的文档    
#### 举例  
``` 
{
  $unwind:
    {
      path: <field path>,
      includeArrayIndex: <string>,  #可选,一个新字段的名称用于存放元素的数组索引。该名称不能以$开头。
      preserveNullAndEmptyArrays: <boolean> #可选，default :false，若为true,如果路径为空，缺少或为空数组，则$unwind输出文档

 } 
}
db.getCollection('test').aggregate(
 [ { $unwind : "$sizes" } ]
)
```
### $group
#### 释义
按指定的表达式对文档进行分组，并将每个不同分组的文档输出到下一个阶段。输出文档包含一个_id字段，该字段按键包含不同的组。

输出文档还可以包含计算字段，该字段保存由$group的_id字段分组的一些accumulator表达式的值。 $group不会输出具体的文档而只是统计信息。
#### 语法
{ $group: { _id: <expression>, <field1>: { <accumulator1> : <expression1> }, ... } }    
解析：_id字段必填，可以指定为null，表示为整个输入文档计算累计值，剩余的计算字段是可选的，并使用<accumulator>运算符进行计算。
#### accumulator操作符

| 名称 |描述 |类比sql
| --- | --- | --- |
| $avg	        |计算均值     	                                                            |avg
| $first        |返回每组第一个文档，如果有排序，按照排序，如果没有按照默认的存储的顺序的第一个文档。	|limit 0,1
| $last	        |返回每组最后一个文档，如果有排序，按照排序，如果没有按照默认的存储的顺序的最后个文档。	|-
| $max	        |根据分组，获取集合中所有文档对应值得最大值。	                                    |max
| $min	        |根据分组，获取集合中所有文档对应值得最小值。	                                    |min
| $push	        |将指定的表达式的值添加到一个数组中。	                                        |-
| $addToSet 	|将表达式的值添加到一个集合中（无重复值，无序）。	                                |-
| $sum	        |计算总和	                                                                |sum
| $stdDevPop	|返回输入值的总体标准偏差（population standard deviation）	                    |-
| $stdDevSamp	|返回输入值的样本标准偏差（the sample standard deviation）	                    |-

#### 注意事项
（1）$group阶段的内存限制为100M，默认情况下，如果stage超过此限制，$group将产生错误，但是，要允许处理大型数据集，需要将allowDiskUse选项设置为true以启用$group操作写入临时文件    
（2）"$addToSet": expr如果当前数组中不包含expr，那就将它添加到数组中    
（3）"$push":expr，不管expr的值，都将它添加到数组中，返回包含所有值的数组。
#### 举例
```  
db.getCollection('test').aggregate([
    {
        $group: {
            _id: {month: {$month: "$data"}, day: {$dayOfMonth: "$date", year: { $year: "$date"}}},
            totalPrice: {$sum: {$multiply: ["$price", "$quantity"]}},
            averageQuantity: {$avg: "$quantitu"},
            count: {$sum: 1}    
        }
    }
])
```
#### 数据转换
（1）可以根据分组把每组转换成item数组    
``` 
db.getCollection('test').aggregate([{$group: {_id: '$price', items: {$pish: "$item"}}}])
```
返回值：    
``` 
{["_id": 5, "items": ["abc", "abc"]]}
```
(2) 可以使用系统变量$$ROOT按item对文档进行分组，生成的文档不得超过BSON文档大小限制    
``` 
db.getCollection('test').aggregate([{$group: {_id: "$item", books: {$push: "$$ROOT"}}}])
```
返回值：    
``` 
/* 1 */
{
    "_id" : "xyz",
    "books" : [ 
        {
            "_id" : 3,
            "item" : "xyz",
            "price" : 5,
            "quantity" : 10,
            "date" : ISODate("2014-03-15T09:00:00.000Z")
        }, 
        {
            "_id" : 4,
            "item" : "xyz",
            "price" : 5,
            "quantity" : 20,
            "date" : ISODate("2014-04-04T11:21:39.736Z")
        }
    ]
}

/* 2 */
{
    "_id" : "jkl",
    "books" : [ 
        {
            "_id" : 2,
            "item" : "jkl",
            "price" : 20,
            "quantity" : 1,
            "date" : ISODate("2014-03-01T09:00:00.000Z")
        }
    ]
}

/* 3 */
{
    "_id" : "abc",
    "books" : [ 
        {
            "_id" : 1,
            "item" : "abc",
            "price" : 10,
            "quantity" : 2,
            "date" : ISODate("2014-03-01T08:00:00.000Z")
        }, 
        {
            "_id" : 5,
            "item" : "abc",
            "price" : 10,
            "quantity" : 10,
            "date" : ISODate("2014-04-04T21:23:13.331Z")
        }
    ]
}
```
### count
返回包含到输出文档的计数
### sort
对文档进行排序，按照排序顺序返回管道。    
1:升序排列    
-1:降序排列    
{$meta：“textScore”}按照降序排列计算出的textScore元数据，表达方式唯一，尽管可以在管道中接受表达式，但{ $meta: "textScore" }表达式仅在包含具有$text查询的$match阶段的管道中有意义。    
分析：$text文本搜索，会为包含索引字段中的搜索词的每隔文档匹配一个分数，这个分数表示文档与给定文本搜索查询的相关性
举例：    
```
db.users.aggregate(
   [
     { $match: { $text: { $search: "operating" } } },
     { $sort: { score: { $meta: "textScore" }, posts: -1 } }
   ]
)
```
### $sortByCount
v3.4新增，根据表达式的值对传入文档分组，计算每个不同组中文档的数量，每个输出文档都包含两个字段：包含不同分组值得_id字段和包含属于该分组或类别的文档数的计数字段，文件按降序排列    
举例：    
``` 
 db.db_test.aggregate({$sortByCount: '$status'});
```
### geoNear
### lookup
mongodb在大部分情况下是不需要连表的，但是依然支持连表操作，即:$lookUp
### out
### indexStats

## 索引
## mongodb正则匹配
## mongodb连接方式
mongodb://[username:password@]host1[:port1][,host2[:port2],...[,hostN[:portN]]][/[database][?options]]
## mongodb查看版本
mongod --version
