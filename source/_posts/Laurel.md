---
title: 一些php知识
date: 2019-03-28 14:00:40
tags:
---
## argc和argv
$argv代表传递的参数,$argc代表参数个数（相对路径+传的参数）
**示例**
```php
<?php
echo $argv[0];     echo "\n";
var_dump($argv[1]);    echo "\n";
var_dump(intval($argv[2]));  echo "\n";
echo $argv[3];     echo "\n";
echo $argc;
```
