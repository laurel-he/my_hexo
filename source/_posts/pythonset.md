---
title: python数据结构之集合
date: 2019-03-15 13:52:00
categories: Python
---
**介绍**
集合是一个无序的不重复的元素序列
**操作**
创建集合：
{}或set()，创建一个空集合必须用set()，{}创建空字典(set("asdgf")会创建['a', 's', 'd', 'g', 'f'],set()创建多个元素需要两个())
parame = {value01,value02,...}或set(value)
添加元素：
s.add(x)
s.update(x)    ->  参数可以是列表，元组，字典等，x可以有多个，用逗号分开
```
thisset = set(("Google", "Runoob", "Taobao"))
thisset.update({1,3})
print(thisset)
{1, 3, 'Google', 'Taobao', 'Runoob'}
thisset.update([1,4],[5,6])  
print(thisset)
{1, 3, 4, 5, 6, 'Google', 'Taobao', 'Runoob'}
```
移除元素：
s.remove(x)   将元素x从集合x移除，如果元素不存在则会发生错误
s.discard(x)  将元素x从集合x移除，如果元素不存在不会发生错误
计算集合元素个数：
len(s)
```
thisset = set(("Google", "Runoob", "Taobao"))
len(thisset)
```
返回3
清空集合：
s.clear()
判断元素x是否在集合s中：
x in s
**内置方法**

| 方法 | 描述 | 
| --- | --- |
| add() | 为集合添加元素 |
| clear() | 移除集合中的所有元素 |
| copy() | 拷贝一个集合 |
| difference() | 返回多个集合的差集 |
| difference_update() | 移除集合中的元素，该元素在指定的集合也存在 |
| discard() | 删除集合中指定的元素 |
| intersection() | 返回集合的交集 |
| intersection_update() | 删除集合中的元素，该元素在指定的集合中不存在 |
| isdisjoint() | 判断两个集合是否包含相同的元素，如果没有返回 True，否则返回 False |
| issubset() | 判断指定集合是否为该方法参数集合的子集 |
| issuperset() | 判断该方法的参数集合是否为指定集合的子集 |
| pop() | 随机移除元素 |
| remove() | 移除指定元素 |
| symmetric_difference() | 返回两个集合中不重复的元素集合 |
| symmetric_difference_update() | 移除当前集合中在另外一个指定集合相同的元素，并将另外一个指定集合中不同的元素插入到当前集合中 |
| union() | 返回两个集合的并集 |
| update() | 给集合添加元素 |
