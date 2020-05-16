---
title: python debugger
date: 2019-03-14 11:48:10
categories: Python
---
##准备
**来源**
http://python.jobbole.com/81184/
**初始代码**
```
import sys
def add(num1=0, num2=0):
    return int(num1) + int(num2)
def sub(num1=0, num2=0):
    return int(num1) - int(num2)
def main():
    #Assuming our inputs are valid numbers
    print sys.argv
    addition = add(sys.argv[1], sys.argv[2])
    print addition
    subtraction = sub(sys.argv[1], sys.argv[2])
    print subtraction
if __name__ == '__main__':
    main()
```
## 基础用法
```
import pdb
pdb.set_trace() //设置断点
```
在程序中添加断点
```
import pdb
import sys
def add(num1=0, num2=0):
    return int(num1) + int(num2)
def sub(num1=0, num2=0):
    return int(num1) - int(num2)
def main():
    #Assuming our inputs are valid numbers
    print sys.argv
    pdb.set_trace() # <-- Break point added here
    addition = add(sys.argv[1], sys.argv[2])
    print addition
    subtraction = sub(sys.argv[1], sys.argv[2])
    print subtraction
if __name__ == '__main__':
    main()
```
## 触发调试器
程序会自动在断点处停止执行
**下一行n**
输入n运行到下一行
**打印p**
可以使用c使pdb跳到末尾或者直到下一个断点
如果想知道sys.argv包含的内容，可以输入p sys.argv
使用这种方法可以方便地查看变量中实际储存着什么值
**单步s**
可以使用s进入函数内部
r返回到进入函数的返回语句
**动态添加断点b**
b 18:代表在18行设置一个断点
**列表l**
l可以显示出此刻在代码中的位置
**结束**
q
**w**
w:显示当前正在执行的代码行的上下文信息
**a**
打印当前函数的参数列表
