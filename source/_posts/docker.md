---
title: docker使用心得
date: 2019-04-24 21:03:10
tags: 使用心得
categories: docker
---
## 准备工作
#### 1 前提条件
ubuntu内核版本高于 3.10  ```uname -r ```

#### 2 安装docker
本文是基于ubuntu的，基于centos安装路径博客如下：
[centos](http://www.runoob.com/docker/centos-docker-install.html "centos")
[windows](http://www.runoob.com/docker/windows-docker-install.html "windows")
```
wget -qO- https://get.docker.com/ | sh
```
当要以非root用户可以直接运行docker时，需要执行 sudo usermod -aG docker runoob 命令，然后重新登陆
启动docker服务
```
sudo service docker start
```
**测试运行hello world**
```
docker run hello-world
```
**镜像加速**
路径：``` /etc/docker/daemon.json```
配置文件中加入(不存在则创建，我本地不存在此文件)：
```
{
  "registry-mirrors": ["http://hub-mirror.c.163.com"]
}
```
## 基础知识
**hello world**
```
docker run ubuntu:15.10 /bin/echo "Hello world"
```
(验证,echo是可以直接echo的，不必写/bin/echo)
**参数解析**
docker:docker的二进制执行文件
run:与前面docker组合来运行一个容器
ubuntu:15.10 : 指定要运行的镜像，Docker首先从本地主机上查找镜像是否存在，如果不存在，Docker 就会从镜像仓库 Docker Hub 下载公共镜像。
/bin/echo "Hello world": 在启动的容器里执行的命令
**运行交互式的容器**
通过docker的两个参数 -i -t，让docker运行的容器实现"对话"的能力
```
docker run -i -t ubuntu:15.10 /bin/bash
```
-t:在新容器内指定一个伪终端或终端。
-i:允许对容器内的标准输入进行交互
**启动容器（后台模式）**
```
docker run -d ubuntu:15.10 /bin/sh -c "while true; do echo hello world; sleep 1; done"
```
输出一串长字符，是容器ID,对每个容器来说都是唯一的，我们可以通过容器ID来查看对应的容器发生了什么。
```docker ps```可以查看有容器在运行
CONTAINER ID:容器ID
NAMES:自动分配的容器名称
在容器内使用docker logs命令，查看容器内的标准输出
```docker logs [CONTAINER ID|NAMES]```
**停止容器**
```docker stop [CONTAINER ID|NAMES]```
(亲测不需要sudo就能停止docker,但是docker ps却需要sudo)
##docker容器使用
```docker```命令可以直接查看Docker客户端的所有选项
```docker comman --help```更深入地了解指定地docker命令使用方法
