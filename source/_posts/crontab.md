---
title: crontab基本使用
date: 2019-04-10 18:27:44
categories: 
- linux
- crontab
---
## 1 简介
crond 是linux下用来周期性的执行某种任务或等待处理某些事件的一个守护进程，与windows下的计划任务类似，当安装完成操作系统后，默认会安装此服务 工具，并且会自动启动crond进程，crond进程每分钟会定期检查是否有要执行的任务，如果有要执行的任务，则自动执行该任务。
## 2 格式
minute hour day month week command
## 3 操作
crontab -l:列出所有任务

## laravel 定时任务
**原文路径**
```https://learnku.com/docs/laravel/5.4/scheduling/1257```
## 其他解决方式
1 supervisor
**举例**
```
[program:finance_svcs_autoreconciliation]
command=php /opt/app/nginx/html/qa6/financesvcs/artisan queue:work beanstalkd --queue=autoreconciliation_broadcast --daemon
directory=/opt/app/nginx/html/qa6/financesvcs
autostart=true
autorestart=true
user=apache
numprocs=1
redirect_stderr=true
stdout_logfile=/var/log/supervisor/qa1/%(program_name)s-stdout.log
stdout_logfile_maxbytes=10MB
stdout_logfile_backups=10
```
```

```
