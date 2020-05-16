---
title: 单例模式
date: 2019-10-29 17:40:10
tags: 学习
categories: 设计模式
---
# 概念介绍
## 情景介绍
一个类只需要被实例化一次，但是不应该由调用方来判断是否被实例化过
## 基础介绍
所有类都有构造方法，不编码则系统默认生成空的构造方法，若有显示定义的构造方法，默认的构造方法就会失效。    
类把构造方法设置为私有，使所有的方法都不能直接实例化这个类。然后类内部写一个公共方法来判断这个类是否被实例化过，调用方无需判断
## 单例模式
单例模式：保证一个类仅有一个实例，并提供一个访问它的全局访问点。    
通常可以让一个全局变量使得一个对象被访问，但它不能防止实例化多个对象。一个最好的办法就是，让类自身负责保存它的唯一实例，这个类可以保证没有其他实例可以被创建，并且它可以提供一个访问该实例的方法。    