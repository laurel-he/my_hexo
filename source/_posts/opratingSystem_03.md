---
title: 操作系统（3）
date: 2021-07-20 16:34:10
tags: 
- 学习
categories: 
- 操作系统
---
# Linux实现模拟多进程并发执行
示例：
```bash
#!/bin/bash
for((i=0;i<5;i++))
do
{
	sleep 1; echo "$i--">>aa && echo "done"
}&
done
wait
cat aa | wc -l
rm aa
```
**解析**
&代表并发执行，wait等待前面执行完了再执行，例如此示例中，如果没有wait很可能还没创建aa文件
# Linux下的中断
```c
#include <stdlib.h>
#include <stdio.h>
#include <unistd.h>

int main(void)
{
	pid_t pid; //pid

	printf("Before fork...
");

	switch( pid = fork()) {
		case -1:
			printf("fork call fail
");
			fflush(stdout);
			exit(1);
		case 0:
			printf("child call
");
			printf("the pid of child is %d
", getpid());
			printf("the pid of child's parent is %d
", getppid());
			printf("child exiting...
");
			exit(0);
		default:
			printf("this is a father
");
			printf("the pid of parent is %d
", getpid());
			printf("the pid of child is %d
", pid);
	}
	printf("After fork, programing exiting
");
	exit(0);

}

```