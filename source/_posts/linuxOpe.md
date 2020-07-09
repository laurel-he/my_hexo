---
title: linux命令大全
date: 2020-02-27 18:00:00
tags: 
- 学习
categories: 
- 操作系统
- linux
- 系统相关
---
## 系统信息
### arch
**描述**

显示机器的处理器架构
**举例**

![towrow](/images/linux/arch.png)
### uname
**描述**

显示机器的处理器架构
**参数**

-a或--all 　显示全部的信息。     
-m或--machine 　显示电脑类型。     
-n或-nodename 　显示在网络上的主机名称。    
-r或--release 　显示操作系统的发行编号。     
-s或--sysname 　显示操作系统名称。      
-v 　显示操作系统的版本。     
--help 　显示帮助。     
--version 　显示版本信息。     
**举例**

![towrow](/images/linux/uname.png)

### dmidecode
**描述**

显示硬件信息，不常用，可忽略
## 开关机
### shutdown
**描述**

关机或重启    
**参数**

-t seconds : 设定在几秒钟之后进行关机程序。    
-k : 并不会真的关机，只是将警告讯息传送给所有使用者。    
-r : 关机后重新开机。    
-h : 关机后停机。    
-n : 不采用正常程序来关机，用强迫的方式杀掉所有执行中的程序后自行关机。    
-c : 取消目前已经进行中的关机动作。    
-f : 关机时，不做 fcsk 动作(检查 Linux 档系统)。    
-F : 关机时，强迫进行 fsck 动作。     
time : 设定关机的时间。    
message : 传送给所有使用者的警告讯息。   
**举例**

```
shutdown -h now //立即关机
shutdown -r now //立即重启
``` 
### reboot
**描述**

重启    
**参数**

-n : 在重开机前不做将记忆体资料写回硬盘的动作     
-w : 并不会真的重开机，只是把记录写到 /var/log/wtmp 档案里     
-d : 不把记录写到 /var/log/wtmp 档案里（-n 这个参数包含了 -d）     
-f : 强迫重开机，不呼叫 shutdown 这个指令     
-i : 在重开机之前先把所有网络相关的装置先停止     
### logout
**描述**

登出系统
###