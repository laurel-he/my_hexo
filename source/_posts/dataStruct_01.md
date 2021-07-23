---
title: 数据结构学习笔记-1
date: 2021-07-20 16:34:10
tags: 
- 学习
categories: 
- 学习
- 数据结构
---
数据结构学习系列都来自于中国大学MOOC上的浙江大学数据结构学习课程。原课程代码全部采用C语言实现，由于本菜平时最擅长（zhihuiyong）使用php，因此所有算法全部翻译成了php实现。课程链接：[数据结构](https://www.icourse163.org/learn/ZJU-93001?tid=1464647442#/learn/announce)    
全文已同步发布到[CSDN](https://blog.csdn.net/qq_28391061/article/details/118090942)
## 定义
（1）数据结构是数据对象，以及存在于该对象的实例和组成实例的数据元素之间的各种联系。这些联系可以通过定义和相关的函数来给出    
（2）数据结构是ADT(abstruct data type)的物理实现    
（3）数据结构是计算机存储，组织数据的方式，通常情况下，精心选择的数据结构可以带来最有效率的算法
## 示例
### 写程序实现一个函数printN，打印从1-N之间的所有正整数
。以下是两种实现方式的php实现代码：
```php
<?php
function printN($n)
{
    $i = 0;
    for($i = 0; $i < $n; $i++)
        {
        echo $i. "\n\r";
}
}
$begin1 = microtime();
printN(10000);
$end1 = microtime();
$time1 = $end1 - $begin1;


function printM($m)
{
    if ($m) {
        printM($m - 1);
        echo $m. "\n\r";
}
}
$begin2 = microtime();
printM(10000);
$end2 = microtime();
$time2 = $end2 - $begin2;

echo $time2 - $time1;
```
递归和直接循环打印都可以实现，但是递归当数字大于1w左右就失败了，因为递归对空间的占用十分恐怖。解决问题方法的效率，也与空间的占用效率是有关的。
### 写程序计算给定多项式在给定点x处的值
多项式：f(x) = a₀ + a₁x + a₂x² ... + aₙ₋₁xⁿ⁻¹ + aₙxⁿ    
使用php的两种实现方式：
```php
<?php

function getPoly(int $n, array $a, float $x)
{
    $res = $a[0];
    for ($i = 1; $i <= $n; $i++) {
        $res += $a[$i] * pow($x, $i);
    }
    return $res;
}
$poliArr = [1, 2, 3, 4, 5];
$n = count($poliArr) - 1;
$x = 0.5;

function getPoly2(int $n, array $a, float $x)
{
    $p = $a[$n];
    for ($i = $n; $i > 0; $i--)
    {
        $p = $a[$i - 1] + $x * $p;
    }
    return $p;
}
echo getPoly($n, $poliArr, $x);
echo "\n\r";
echo getPoly2($n, $poliArr, $x);
```
getPoly比getPoly2的运行速度差了接近一个数量级，说明解决问题方法的效率，跟算法的精妙程度有关。
## 总结
数据结构：数据在计算机中的组织方式    
数据对象必定与一系列加在其上的操作相关联，完成这些操作所用的方法叫算法    
### 抽象数据类型(Abstract Data Type)
#### 数据类型
数据对象集、数据集合相关联的操作集
#### 抽象
描述数据类型的方法不依赖于具体实现，与存放数据的机器无关，与数据存储的物理结构无关，跟实现操作的算法和编程语言均无关，只描述数据类型是什么，不涉及如何做到的问题
### 算法
（1）一个有限指令集    
（2）接受一些输入（有时不需要输入）    
（3）产生输出    
（4）一定在有限步骤之后终止    
（5）每一条指令必须    
a 有充分明确的目标，不可以有歧义     
b 要在计算机能处理的范围之内      
c 描述应不依赖于任何一种计算机的语言以及具体的实现方式
#### 什么是好的算法
使用空间复杂度S(n)和时间复杂度T(n)来衡量     
在分析一般算法的效率时，通常关心两个复杂度：最坏情况复杂度，平均复杂度
