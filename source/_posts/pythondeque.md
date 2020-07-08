---
title: python数据结构之双向队列
date: 2019-03-15 09:47:00
categories: 
- 服务端语言
- python
---
**介绍**
两端都可以操作的序列
**基本操作**
创建：
import collections
d = collections.deque()
往右边添加一个元素：
d.append(1)
d.append('adc')
往左添加一个元素：
d.appendleft(3)
清空队列：
d.clear
浅拷贝：
d1 = d.copy()
返回指定元素的出现次数：
d.count(1)
从队列右边扩展一个列表的元素：
d.extend([3,4,5])
从队列左边扩展一个列表的元素：
d.extendleft([3,4,5])
查找某个元素的索引位置：
d.index("c", 0, 2) //指定查找区间
在指定位置插入元素：
d.insert(2,"z")
删除最右边一个元素（返回删除的元素）
d.pop()
删除最左边一个元素（返回删除的元素）
d.popleft()
删除指定元素
d.remove("c")
队列翻转
d.reverse()
把右边元素放左边
d.rotate(2) (每个元素向右移动n个，右边元素往左进)


feature/20190315_auto_reconcile_log
http://finance.administration.dev5.tff.com/reconciliation/download?platform=%E9%A9%AC%E8%9C%82%E7%AA%9D&per_page=683&page=1
