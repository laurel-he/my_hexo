---
title: 运行相关
date: 2020-02-25 20:10:10
tags: 学习
categories: 有用代码段收集
---
#### 1 下载数据
**描述**
根据链接下载文件
**代码**
```php
<?php 
function downloadOldFile($url,$rename,$ext){
    $file_path = 'public/images/age_60-120/';
    $ch = curl_init($url);
    curl_setopt($ch, CURLOPT_HEADER, 0);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
    curl_setopt($ch, CURLOPT_BINARYTRANSFER, 1);
    $rawdata=curl_exec ($ch);
    curl_close ($ch);
    // 使用中文文件名需要转码
    $fp = fopen($file_path. $rename .".".$ext,'w');
    fwrite($fp, $rawdata);
    fclose($fp);
    // 返回路径
    return $_SERVER['DOCUMENT_ROOT'].$file_path.$rename.".".$ext;
}
?>
```
#### 2 按步骤执行方法
**描述**
经常有需求：分页执行脚本，例如前1000条执行完后继续分页，此时就需要自动跳页执行
**代码**
```php
<?php
    /**
     * @param mixed ...$args
     * @return bool
     * @throws Exception
     */
    private function stepRun(...$args)
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
    // 调用
    //     $this->stepRun($exportCount, 'loopChildData', 1000, $ageId);
    
    // 执行方法
    /**
        function loopChildData(int $limit = 1000, int $skip = 0, string $ageId = '')
        {
            $data = $this->dao->conn->query(['faceItems' => ['$in' => [$ageId]], 'status' => 2, 'apiCheckPass' => true],
                null, ['limit' => $limit, 'skip' => $skip]);
            foreach ($data as $v) {
                $this->downloadChildFile(Url::createPrivateUrl($v['url']), $v['_id'], 'jpg');
            }
        }
    **/
?>
``