---
title: 命令模式
date: 2019-10-04 17:40:10
tags: 
- 概念介绍
categories: 
- 学习
- 设计模式
---
# 基础概念
## 基础介绍
（1）概念介绍：    
命令模式（Command Pattern）是一种数据驱动的设计模式，它属于行为型模式。请求以命令的形式包裹在对象中，并传给调用对象。调用对象寻找可以处理该命令的合适的对象，并把该命令传给相应的对象，该对象执行命令。    
（2）概念解析：    
命令模式将一个请求封装成一个对象，从而可用不同的请求对客户进行参数化，对请求排队或记录请求日志，以及支持可撤销的操作。    
## 使用场景
在某些场合下，需要对行为进行"记录、撤销/重做、事务"等处理，需要将"行为请求者"与"行为实现者"解耦，将一组行为抽象为对象，可以实现二者之间的松耦合。认为是命令的地方都可以使用命令模式，比如： 1、GUI 中每一个按钮都是一条命令。 2、模拟 CMD。    
## 关键术语
（1）received：真正的命令执行对象    
（2）Command：命令    
（3）invoker：使用命令对象的入口
## 实现步骤
以下以后台k8s删除node节点命令为例
### a.创建一个命令接口：    
接口定义接口名称，需要实现的操作名称，命令模式需要的操作为执行命令，后续可根据不同的命令来实现这个接口和执行命令方法。
```php
<?php
abstract class Nodes {
   public function execute(){}
}
?>
```
### b.创建一个请求类
定义拥有的请求方法        
本例中操作有：    
（1）列出所有nodes:  kubectl get node    
（2）删除节点：kubectl delete node $nodeName 
（3）查看对应node上的pods信息: kubectl get pods -o wide | grep $nodeName    
（4）在删除的node3对应的服务器上执行：kubeadm reset
```php
<?php
class K8s {
    public function getNodes()
    {
        return 'kubectl get node';
    }
    public function delNodes($nodeName)
    {
        return "kubectl delete node $nodeName ";
    }
}
?>
```
### c.创建实现接口的实体类
针对不同的方法实现命令接口。即实现a步骤（创建一个命令接口）所创建的接口，b中的每个请求方法都需要实现对应的实体类。    
```php
<?php
class GetNodes implements Nodes {
   private $nodes = new K8s(); 
   public function getK8s():K8s
   {
       if (empty($this->nodes)) {
           $this->nodes = new K8s();
       }
       return $this->nodes;
   }
   public function execute()
   {
       $this->getK8s()->getNodes();
   }
}
class DelNodes implements Nodes {
   private $nodes = new K8s(); 
   public function getK8s():K8s
   {
       if (empty($this->nodes)) {
           $this->nodes = new K8s();
       }
       return $this->nodes;
   }
   public function execute()
   {
       $this->getK8s()->delNodes();
   }
}
?>
```
### 创建命令调用类
broker,用来调用命令
```php
<?php
class Broker{
    private $commandList = [];
    public function addList($k8s)
    {
        array_push($this->commandList, $k8s);
    }
    public function executeList()
    {
        foreach ($this->commandList as $command) {
            $command->execute();
        }
    }
}
?>
```
### 使用broker类来接受并执行命令
调用broker类，根据需要（顺序，需要使用的方法等）添加一个执行队列（数组），之后再执行
```php
<?php
class CommandUse
{
    public function executeCommand()
    {
        $getNodes = new GetNodes();
        $delNodes = new DeleNodes();
        
        $broker = new Broker();
        $broker->addList($getNodes);
        $broker->addList($delNodes);
        
        $broker->executeList();
    }
}
?>
```
### 执行程序输出结果
## 命令模式的优点
（1）比较容易设计出一个命令队列    
（2）在需要的情况下，可以比较容易的将命令记入日志    
（3）允许接收请求的一方决定是否要否决请求    
（4）可以容易地实现对请求的撤销和重做    
（5）由于加进新的具体命令类不影响其他的类，因此增加新的具体命令类很容易    
（6）命令模式把请求一个操作的对象和知道怎么执行一个操作的对象分割开    