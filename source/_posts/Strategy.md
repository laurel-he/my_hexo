---
title: 策略模式
date: 2019-07-29 14:37:10
tags: 概念介绍
categories: 
- 设计模式
---
## 概念
### 定义
策略模式定义了算法族，分别封装起来，让他们之间可以相互替换，该模式让算法独立于使用它的客户而独立变化
### 组成
1 抽象策略角色：策略类，通常由一个接口或者抽象类实现    
2 具体策略角色：包装了相关的算法和行为。     
3 环境角色：持有一个策略类的引用，最终给客户端调用。
#### 分析
策略模式用一个抽象策略角色提供一个类（一般是接口或抽象类），每个策略都实现了这个抽象策略角色，环境角色去调用（依赖注入）    
#### 代码
```php
<?php
/**
 * @author: hexiaojiao@jiapinai.com
 * @todo:
 * time: 2019-07-30 20:07
 */

/**
 * 定义抽象角色类`
 * Interface CollectInter
 */
interface CollectInter
{
    public function collect($price, $num);
    public function input();
}
class Base
{
    public function input($discount = 1)
    {
        fwrite(STDOUT, '请输入单价');
        $price = trim(fgets(STDIN));
        fwrite(STDOUT, '请输入数量');
        $num = trim(fgets(STDIN));
        $res = $this->collect($price, $num, $discount);
        return $res;
    }
}

/**
 * 定义具体策略类
 * Class Collect02
 */
class Collect02 extends Base implements CollectInter
{
    public function collect($price, $num, $discount = 1) {
        var_dump('Collect02:', $price * $num * $discount);
        return true;
    }
}

/**
 * Class Discount
 */
class Discount extends Base implements CollectInter
{
    public function collect($price, $num, $discount = 0.8)
    {
        var_dump('Discount:', $price * $num * $discount);
        return true;
    }
}

/**
 * Class Reduce
 */
class Reduce extends Base implements  CollectInter
{
    public function collect($price, $num, $total = 100, $reduce = 0)
    {
        if ($price * $total >= $total) {
            var_dump('Reduce:', ($price * $total) - $reduce);
        } else {
            var_dump('Reduce:', $price * $total);
        }
        return true;
    }
}
/**
 * 环境角色类
 * Class Main
 */
class Main
{
    private $_strategy;
    private $_isChange;
    public function __construct(CollectInter $collectInter)
    {
        $this->_strategy = $collectInter;
    }
    public function change(CollectInter $collectInter)
    {
        $this->_strategy = $collectInter;
        $this->_isChange = true;
    }
    public function beginCollect()
    {
        if ($this->_isChange) {
            echo "改变收银方式：";
            $this->_strategy->input();
        } else {
            $this->_strategy->input();
        }
    }
}
$strategy = new Main(new Discount());
$strategy->beginCollect();
$strategy->change(new Collect02());
$strategy->beginCollect();
```
#### 运行结果
```
请输入单价4
请输入数量2
string(9) "Discount:"
int(8)
改变收银方式：请输入单价7
请输入数量3
string(10) "Collect02:"
int(21)
```
