---
title: react学习-javascript函数式编程
date: 2019-07-09 09:37:10
tags:
categories: 
- 前端
- react
---
# react学习手册
## Javascript函数式编程
### 基础概念
1 函数式编程：    
函数是第一类成员，函数就是数据，可以像变量那样被保存，检索或者在应用程序内部传递。    
（1）可以像声明变量一样声明函数；函数就是变量，可以添加到对象中：
```javascript
const obj = {
    message: 'asdsada',
    log(message) {
      console.log(message);
    }
  };
  obj.log(obj.message);
```
（2）可以将函数添加到数组中；    
（3）函数可以像其他变量一样，作为其他函数的参数传递，也可以作为其他函数的执行结果被返回;    

### 命令式和声明式编程
命令式编程：对执行结果的描述胜于执行过程    
声明式编程： 代码关注重点是达成目标的具体过程
### 不可变性
（1）使用Objct.assign()方法：一种拷贝机制，提供一个空白对象，将操作的对象拷贝到该对象上，然后修改拷贝对象
```javascript
Object.assign({}, color, {rating:rating})
```
（2）可以使用ES6规范下的箭头函数和ES7规范下的对象扩展运算符编写同样的函数。
```javascript
const rateColor = (color, rating) => ({
    ...color,
    rating
})
```
注意，在箭头函数中，箭头不能指向一个对象的花括号；Array.push方法不是一个不可变函数，使用Array.concat取代    
可以使用ES6扩展运算符串联数组，同时改操作符可以使用同样的机制拷贝对象，并将它添加到原生数组的副本上
```javascript
const addColor = (title, list) => [...list, {title}]
```
### 纯函数
1 概念：    
返回结果值依赖于输入参数的函数。纯函数至少需要接受一个参数并且总是返回一个值或者其他函数。不产生副作用，不修改全局变量或任何应用程序的state,将输入参数当成不可变数据   
```javascript
    var frederick = {
      name: "Frederick Douglass",
      canRead: false,
      canWrite: false
    }

    const selfEducate = person =>
        ({
            ...person,
            canRead: true,
            canWrite: true
        })

    //
    // Pure Function, takes an argument, returns an object,
    //    and causes zero side effects
    //

    console.log( selfEducate(frederick) )
    console.log( frederick )
```
2 纯函数三条原则：    
（1）函数应该至少接收一个参数；    
（2）函数应该返回一个值或者其他函数；    
（3）函数不应该修改或者影响任何传给它的参数
### 数据转换
1 概念：    
将数据转换成另外一种数据，使用函数生成转换后的副本
2 Array.map和Array.filter方法    
转换数组    
3 reduce和reduceRight    
可以用来将数组转换成任意值。    
举例：    
（1）数组转换成数字:以下实例循环迭代，每次返回最大值，然后将结果作为上一次回调函数的返回值
```javascript
function getAge() {
        const ages = [10, 21,33, 20,15, 14, 90, 12];
        const maxAge = ages.reduce((max, age) => {
            console.log(`${age} > ${max} = ${age > max}`);
            if (age > max) {
                return age;
            } else {
                return max;
            }
        }, 0);
        const max = ages.reduce((max, value) => {(value > max) ? value : max}, 0);        
        console.log('maxage', maxAge);
    }
```
reduceRight和reduce类似，从数组末尾处理元素。    
（2）数组转化成对象    
