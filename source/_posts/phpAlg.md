---
title: PHP常见算法
date: 2019-04-08 18:27:44
tags:
categories: 
- php
---
# 排序算法
## 1 冒泡排序
**解析**
从零开始，每一位和剩余位数比较，如果大于（小于）则交换，最后一位最终会是最大（最小）的。
**代码**
```php
<?php
$arr = [2,3,1,4,7,6,9];
var_dump(bubbleSort($arr));
function bubbleSort($arr)
{
    $len = count($arr);
    for($i = 0;$i < $len; $i++) {
        for($j = $len - 1;$j > $i;$j--) {
            if($arr[$j] < $arr[$j-1]) {
                $tmp = $arr[$j];
                $arr[$j ] = $arr[$j - 1];
                $arr[$j - 1] = $tmp;
            }
        }
    }
    return $arr;
}

```
## 2 快速排序
**解析**
选中一个值（一般是$arr[0]）作为基准，通过一次排序分割成比基准数小和比基准数大的两部分
```php
<?php
$arr = [3,4,1,5,7,2,6];
function quickSort($arr)
{
    if(count($arr) <= 1)
    {   
        return $arr;
    }   
    $mid = $arr[0];
    $left = array();
    $right = array();
    for($i = 1;$i<count($arr);$i++)
    {   
        if($mid < $arr[$i])
        {   
            $right[] = $arr[$i];
        } else {
            $left[] = $arr[$i];
        }   
    }   
    $left = quickSort($left);
    $right = quickSort($right);
    return array_merge($left,array($mid),$right);
}
var_dump(quickSort($arr));

```
**注意事项**
array_merge()需要将左边，中间和右边都合并

# 查找算法
## 折半查找

