---
title: 计算相关
date: 2020-02-25 20:10:10
tags: 学习
categories: 有用代码段收集
---
#### 1 计算矩形是否相交
**描述**
给定两个矩形的左上角起始点坐标和矩形的宽，高，计算这两个矩形是否相交    
**分析**
 1 矩形1左上角坐标p1,右下角坐标p2,矩形2左上角坐标p3,右下角坐标p4
 2 M点坐标为P1与P3点在X方向较大值与Y方向较大值的交点
 3 N点坐标为P2与P4点在X方向较小值与Y方向较小值的交点
 4 如果M点的X坐标和Y坐标值均比N点相应的X坐标和Y坐标值小，亦即M和N可以分别构成一个矩形的左上角点和右上角点，则两矩形相交；其余情况则不相交
 **代码**
 ```php
<?php
function checkRectInterSect($rect1, $rect2)
    {
        $x1 = $rect1['x'];   $w1 = $rect1['w'];
        $y1 = $rect1['y'];   $h1 = $rect1['h'];
        $x2 = $rect2['x'];   $w2 = $rect2['w'];
        $y2 = $rect2['y'];   $h2 = $rect2['h'];
        $p1 = [$x1, $y1];
        $p2 = [$x1 + $w1, $y1 + $h1];
        $p3 = [$x2, $y2];
        $p4 = [$x2 + $w2, $y2 + $h2];

        $M = [max($p1[0], $p3[0]), max($p1[1], $p1[1])];
        $N = [min($p2[0], $p4[0]), min($p2[1], $p4[1])];

        if ($M[0] < $N[0] && $M[1] < $N[1]) {
            return true;
        } else {
            return false;
        }
    }
?>
```
#### 2 json转csv
**描述**
需要将数据库数据导出为csv文件
**代码**
```php
<?php
    function getCsvData(int $limit = 1000, int $skip = 0, string $ageId = '')
    {
        $data = $this->dao->conn->query(['faceItems' => ['$in' => [$ageId]], 'status' => 2, 'apiCheckPass' => true],
            null, ['limit' => $limit, 'skip' => $skip]);
        foreach ($data as $v) {
            foreach ($v as $key => $values) {
                if (is_array($values)) {
                    $v[$key] = implode('|', $values);
                }
                (string)$v[$key];
            }
            $exportData = implode(',', $v);
            $exportData = $exportData . "\r\n";
            $myfile = fopen("data.csv", "a") or die("Unable to open file!");
            $this->writeToCsvFile($data, $myfile, $exportData);
            fclose($myfile);
        }
    }
    function writeToCsvFile(array $data, $myfile, $exportData)
    {
//        $txt = \GuzzleHttp\json_encode($data);
        fwrite($myfile, $exportData);
        return true;
    }
?>
```
