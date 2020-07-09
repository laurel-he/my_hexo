---
title: 抽象工厂模式
date: 2019-07-22 17:40:10
tags: 
- 概念介绍
categories: 
- 学习
- 设计模式
---
## 目的
在不指定具体类的情况下创建一系列相关或依赖对象。 通常创建的类都实现相同的接口。 抽象工厂的客户并不关心这些对象是如何创建的，它只是知道它们是如何一起运行的。
## 举例
#### 
```php
<?php
class KuaidiBirdLogic extends BaseLogic
{
    private $basic;
    private $num;
    public function __construct(int $num, int $basic)
    {
        $this->num = $num;
        $this->basic = $basic;
    }
}
class KuaidiOneLogic implements KuaidiBase
{
    private $num;
    public function __construct(int $num)
    {
        $this->num = $num;
    }
}
```
定义抽象工厂类
```php
<?php
class ExpressFactory extends Model
{
    public function getKuaidiBirdList() {
        return new KuaidiBirdLogic(12, 13);
    }
    public function getKuaidiOneLogic() {
        return new KuaidiOneLogic();
    }
}

```
## 几种工厂模式的区别
#### 抽象工厂模式
类似于示例Basic
#### 简单工厂模式
#### 工厂方法模式
#### 静态工厂模式
