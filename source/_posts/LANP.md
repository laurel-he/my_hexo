---
title: virtualbox搭建LANP环境（ubuntu）
date: 2019-04-28 14:00:40
tags:
---
## 1 基础软件下载
virtual box
ubuntu(ubuntu server)
## 2 安装ubuntu
(1)virtualbox创建虚拟电脑
注意：最好重新设置virtualbox->管理->全局设定->默认虚拟电脑位置
(2)启动，网络设置成网络地址转换(NAT)
(3)重启的时候需要弹出U盘或者改变镜像位置
(4)如果提示未指定nat网络名称，管理->全局设定->网络->nat网络（添加一条）
(5)
## 3 配置ssh
(1)添加端口映射（例如2222->22）,注意主机端口不可重复，且子系统ip不填
(2)ubuntu安装ssh
```
sudo apt-get install openssh-server
```
(3)xshell等工具连接，端口号为转发的主机端口
## 4 安装php nginx Mysql
(1)安装php
```
sudo apt-get install php7.0 
```
(2)安装nginx
```
sudo apt-get install nginx
```
(3)安装Mysql
```
sudo apt-get install mysql-server-5.7 mysql-client-5.7
```
## 5 基础配置
(1)安装PHPFastCGI管理器
sudo apt install php7.2-fpm

## 6其他tips
(1)配置sudo
修改 /etc/sudoers，末尾添加一行：
``` 
test    ALL=(ALL) NOPASSWD: ALL
```
test表示用户名；
保存退出后执行一下命令：
``` 
sudo usermod -aG test test
```
(2)配置共享文件夹且自启动
1 virtualbox(设置-共享文件夹-勾选固定分配(不勾选自动挂载))
2 ubuntu创建文件夹
3 挂载共享文件夹
``` 
sudo mount -t vboxsf filesname /mnt/filesname
```
**报错**
可能报错wrong fs type, bad option, bad superblock
，经过搜索需要安装```apt-get install nfs-common```   
但是安装后依旧报错，查看系统日志，报错信息如下：
![explain](/images/mysqlIndex/error.png)
只查到了一条有用的记录，且是英文的，原来这是virtualbox的一个bug，我记得virtualbox还有一个bug,就是不能在共享文件夹里使用python的虚拟环境，但是这版直接共享文件夹都不能挂载了。路径如下：
[virtualbox bug](https://www.virtualbox.org/ticket/9307 "virtualbox bug")
再安装两个插件即可
``` 
sudo apt install cifs-utils
sudo apt install virtualbox-guest-utils
```
4 共享文件挂载自启动
(3)配置rsa
1 windows生成私钥
```
ssh-keygen -t rsa -C <your_email@example.com>
```
2 git配置
在git配置上加上公钥
3 linux配置对应rsa
进入当前用户的home目录(~)，创建.ssh文件夹,复制私钥到文件夹下，进入项目目录，即可执行git clone(可能权限不够，可以暂时把项目目录设置成777)
## 7 配置
#### 1 配置nginx
```
sudo vim /etc/nginx/sites-enabled/default
//需要给默认index加上index.php
```
