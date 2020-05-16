---
title: 有用的代码段-php篇
date: 2020-01-16 12:34:10
tags: tips
categories: 工作经验
---
# 分步骤执行
```php
<?php
/**
 * @param mixed ...$args
 * @return bool
 * @throws Exception
 */
public function stepRun(...$args): bool
{
    $count = $args[0];
    $method = $args[1];
    $step = array_key_exists(2, $args) ? $args[2] : 10;
    $limit = array_key_exists(3, $args) ? $args[3] : 10;
    $param = array_key_exists(4, $args) ? $args[4] : '';
    $stepCount = ceil($count / $step);
    for ($nowStep = 0; $nowStep < $stepCount; $nowStep ++) {
        $this->$method($limit, $nowStep * $step, $param);
        sleep(1);
    }
    sleep(1);
    return true;
}
?>
```
## 使用方法
```php
<?php
$this->stepRun($groupChangeCount, 'loopGetData', 30, 30);
?>
```
## 分析
可运用于需要遍历数据库数据，对数据执行操作的功能上。
# mongodb查询
```php
<?php
/** 宜宾爱你宝贝
 * @return bool|int
 * @throws \yii\base\InvalidConfigException
 */
public function checkAssign()
{
    $query = [
        '$or' => [
            ['isMarked' => false],
            ['isMarked' => ['$exists' => false]]
        ],
        'faceChecked' => true,
        'assignAuthor' => BackendCookieHelper::getName()];
    if ($this->dao->conn->count($query) >= 20) {
        return true;
    } else {
        unset($query['assignAuthor']);
        return $this->assignToPerson($query);
    }
}
?>
```