---
title: python进阶
date: 2019-03-14 10:36:10
categories: Python
---
# *args和**kwargs
## *args
*args和**kwargs主要用于函数定义，可以将不定数量的参数传递给一个函数
**举例**
输入：
```
def test_var_args(f_arg, *argv):
print("first normal arg:", f_arg)
for arg in argv:
print("another arg through *argv:", arg)
test_var_args('yasoob', 'python', 'eggs', 'test')
```
输出：
```
first normal arg: yasoob
another arg through *argv: python
another arg through *argv: eggs
another arg through *argv: test
```
## *kwargs
*kwargs允许将不定长度的键值对作为参数传递给一个函数，如果想在一个函数里处理带名字的参数，应该使用**kwargs
使用示例如下：
```
def getArgs(f_arg, *args):
    print "first is" + f_arg
    for arg in args:
        print "the data is" + arg 
def gree(**kwa):
    for key, value in kwa.items():
        print("{0} == {1}".format(key,value))
#getArgs('name1', 'name2', 'name3', 'name4')
gree(name='test', age=12, first="apple")
```
输出：
```
age == 12
name == test
first == apple
```
## 使用*args和**kwargs来调用函数
如果存在以下函数：
```
def test_args_kwargs(arg1, arg2, arg3):
print("arg1:", arg1)
print("arg2:", arg2)
print("arg3:", arg3)
```
可以使用*args或**kwargs来给这个函数传递参数
(1)如果是*args
```
args = ("two", 3, 5)
test_args_kwargs(*args)
```
输出：
```
arg1: two
arg2: 3
arg3: 5
```
(2)如果是**kwargs
```
kwargs = {"arg3": 3, "arg2": "two", "arg1": 5}
test_args_kwargs(**kwargs)
```
输出：
```
arg1: 5
arg2: two
arg3: 3
```
(3)标准参数与*args、*kwargs在使用时的顺序
some_func(fargs, *args, **kwargs)
#### 使用场景
**函数装饰器**
**猴子补丁**

##调试**
[python debugger](https://www.hexiaojiao.top/2019/03/14/python_debugger/ "python debugger")
##生成器和迭代器
##Map Filter和Reduce

