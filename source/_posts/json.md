---
title: 生成json映射表
date: 2019-08-15 10:27:44
categories: 
- 工作
---
## 生成json映射表代码
```php
<?php
$dic = [
            'qd' => [...],
            'mp' => [... 100],
            'jl' => [...],
            'sb' => [..., 50, 60, 70, 85, 100],
            'ly' => [..., 50, 60, 70, 85, 100],
            'dy' => [...0, 50, 60, 70, 85, 100],
            'sl' => [...55, 60, 65, 70, 85, 100],
            'xl' => [... 50, 55, 65, 75, 85, 100],
            'vl' => [...50, 60, 70, 80, 90, 100],
            'rh' => [... 100]
        ];
function getJson($dic) {
    $res = [];
foreach($dic as $k => $v){
    $res[$k] = [];
    foreach($v as $key => $val) {
        $res[$k]['mapV'][$key] = [
            'label' => (string)$key,
            'value' => (string)$val
];
}
}
var_dump(json_encode($res));
}
getJSON($dic);
?>

```
