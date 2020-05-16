---
title: 《第一本docker书》学习笔记01
date: 2020-04-21 21:03:10
tags: 学习
categories: docker
---
# Docker与配置管理
## docker的核心组件
（1）docker客户端和服务器，也称为docker引擎    
（2）docker镜像     
（3）Registry    
（4）Docker容器
## 基本使用
### docker info
查看docker程序是否存在，功能是否正常
### docker exec(docker run)
docker run 命令提供了docker容器的创建到启动的功能    
(1)-i:保证容器STDIN是开启的     
(2)-t:为要撞见的容器分配一个伪tty终端
docker exec -it backend_jiapin /bi
n/bash