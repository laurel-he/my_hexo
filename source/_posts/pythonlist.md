---
title: python数据结构之列表
date: 2019-03-15 13:47:00
categories: 
- 服务端语言
- python
---
**介绍**

可以作为一个方括号内的逗号分隔值出现，列表的数据项不需要具有相同的类型
**操作**
（1）取值：
list[0], list[0:3] （list[-2]代表读取列表中倒数第二个元素）
（2）更新列表
添加元素：append--> list.append('test')
删除元素：del---> del list[2]
**脚本操作符**

| python表达式 | 结果 | 描述 
| --- | --- | --- |
| len([1,2,3]) | 3 | 长度 |
| [1, 2, 3] + [4, 5, 6] | [1, 2, 3, 4, 5, 6] | 组合 |
| ['Hi!'] * 4 | ['Hi!', 'Hi!', 'Hi!', 'Hi!'] | 重复 |
| 3 in [1, 2, 3] | 	True | 元素是否存在于列表中 |
| for x in [1, 2, 3]: print x, | 1 2 3 | 迭代 |
**函数&方法**

| 函数 | 作用 | 
| --- | --- |
| cmp(list1, list2) | 比较两个列表的元素 |
| len(list) | 列表元素个数 |
| max(list) | 返回列表元素最大值 |
| min(list) | 返回列表元素最小值 |
| list(seq) | 将元组转换为列表 |

| 方法 | 作用 | 
| --- | --- |
| list.append(obj) | 在列表末尾添加新的对象 |
| list.count(obj) | 统计某个元素在列表中出现的次数 |
| list.extend(seq) | 在列表末尾一次性追加另一个序列的多个值（用新列表扩展原来的列表） |
| list.index(obj) | 从列表中找出某个值第一个匹配项的索引位置 |
| list.insert(index, obj) | 将对象插入列表 |
| list.pop([index=-1]) | 移除列表中的一个元素（默认最后一个元素），并返回该元素的值 |
| list.remove(obj) | 移除列表中的某个值的第一个匹配项 |
| list.reverse() | 反向列表中的元素 |
| list.sort(cmp=None, key=None, reverse=False) | 对原列表进行排序 |
**注意事项**
切片：[m:n]从索引m开始，直到索引n为止，但不包括索引n
句尾加逗号：输出在一行中
sort():直接修改原列表，返回None
