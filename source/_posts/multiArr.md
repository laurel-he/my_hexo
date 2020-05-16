---
title: php多维数组相关处理
date: 2020-04-16 18:27:44
tags: 工作经验
categories: 
- php
---
## 使用场景
在工作中遇到了两次需要把不确定维度的多维数组渲染出来的情况，且还需要修改值和结构，如果使用json-patch会受到限制（需要根据不同的情况打补丁，且由于结构不一定一致，打补丁可能会错误地修改某些文件），因此需要自行设计批量修改如yaml,json等类似于多级结构的方法
## 具体解析
### 多维数组转化的json前端渲染
结构不固定,获取对应的json，ts实现
``` 
formatJsonData = (obj: any, name: string, moduleName?: string, getModuleName?: string): any[] => {
  const arr: any[] = []
    if (Object.keys(obj)) {
      let keys = Object.keys(obj);
      let i = 0;
      return keys.map((items) => {
        i++;
        if (obj[items] instanceof Object) {
          let itemRes = items;
          if (obj[items]['type']) {
            itemRes = obj[items]['type'];
          }
          const names = name + '=' + itemRes
          return this.formatJsonData(obj[items], names, moduleName);
        } else {
          const resItems = name + '=' + items;
          this.routesValue.set(resItems, obj[items])
          if (getModuleName && resItems === getModuleName) {
            return obj[items];
          }
        }
      });
    }
    return arr
}

```
结构不固定，根据json获取对应的值，php实现：
```php
<?php
/**
     * 将数组格式化为mongo可用的filter
     * @param array $arr
     * @param string $origin
     * @return array
     */
    private function formatArr2MongoFilter(array $arr, string $origin = 'data'): array
    {
        if (array_keys($arr)) {
            $keys = array_keys($arr);
            foreach ($keys as $key) {
                if (is_array($arr[$key])) {
                    $resName = $origin . '.' . $key;
                    return $this->formatArr2MongoFilter($arr[$key], $resName);
                } else {
                    $resName = $origin . '.' . $key;
                    return ['mongoFilter' => $resName, 'value' => $arr[$key]];
                }
            }
        }
    }
?>
```
## 比较两个多维数组获取差异
```php
<?php
/**
     * 比较多维数组获取差异值
     * @param $array1
     * @param $array2
     * @return int
     */
    private function array_diff_assoc_recursive($array1, $array2)
    {
        foreach($array1 as $key => $value)
        {
            if(is_array($value))
            {
                if(!isset($array2[$key]))
                {
                    $difference[$key] = $value;
                }
                elseif(!is_array($array2[$key]))
                {
                    $difference[$key] = $value;
                }
                else
                {
                    $new_diff = $this->array_diff_assoc_recursive($value, $array2[$key]);
                    if($new_diff != FALSE)
                    {
                        $difference[$key] = $new_diff;
                    }
                }
            }
            elseif(!isset($array2[$key]) || $array2[$key] != $value)
            {
                $difference[$key] = $value;
            }
        }
        return !isset($difference) ? 0 : $difference;
    }
?>
```