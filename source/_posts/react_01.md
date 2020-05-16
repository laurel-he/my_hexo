---
title: react学习-es6基础特性
date: 2019-07-08 09:37:10
tags:
categories: 
- 前端
- react
---
# react学习手册
## 新特性
1 react开发者工具：react developer tools，可以查看网站react元素    
2 yarn进行依赖管理：yarn.lock文件，可以使用yarn install安装，npm install yarn,yarn add [package-name],yarn remove [package-name]
#### 变量声明
1 const:常量，不能被修改，重写会报错；   
2 let:变量，可以将变量作用域限定在任意代码块中（var不行）    
3 模板字符串${}，可在字符串中插入变量
#### 箭头函数
1 介绍：用户可以不使用function关键字创建函数，通常还不需要return    
2 箭头函数可以限制this的作用域，箭头函数没有自己的this,引用外层的this;
#### ES6转译
1 介绍：浏览器运行前将代码转成符合ES5规范的代码，Bable是当前最流行的转译工具之一
#### ES6的对象和数组
1 解构赋值：    
(1)允许用户将某个对象内的字段的作用域本地化，并且声明哪些值是将要使用的。
举例：  
```javascript
 var sandwich =  {
      bread: "dutch crunch",
      meat: "tuna",
      cheese: "swiss",
      toppings: ["lettuce", "tomato", "mustard"]
    }
    var {bread, meat} = sandwich
    console.log(bread, meat)
    bread = "garlic"
    meat = "turkey"
    console.log(bread,meat)
    console.log(sandwich.bread, sandwich.meat)

```
(2)除了使用点符号访问对象内部属性之外，还可以在对象之外解构需要使用的值。    
```javascript
var regularPerson = {
    firstname: "Bill",
    lastname: "Wilson"
    };
    var lordify = ({firstname}) => {
    	console.log(`${firstname} of regularPerson`);
    }
    lordify(regularPerson);
```
(3)数组中的值也可以被解构，还可以使用都好进行列表匹配，跳过不需要的值；    
```javascript
   var [firstResort] = ["Kirkwood", "Squaw", "Alpine"]
    console.log(firstResort)
   var [, , third] = ["Kirkwood", "Squaw", "Alpine"]
   console.log(third)
```
2 对象语义增强    
可以从全局作用域中获得变量并转化成一个对象。还可以通过对象语义增强或重组构造对象的方法：    
```javascript
  let name = 'yaya';
  let ela = 'test';
  let func = {name, ela};
  let test = function() {
    console.log(`Mmt. ${this.name}  is aa ${this.ela} oo`);
  }
  console.log('func', func);
  let funclike = {name, ela, test};
  funclike.test();
```
定义了对象方法后就不需要使用关键字function了
```javascript
// 旧方案
var name = "Léo Taillefer"
    var sound = "Kahh"
    var skier = {
      name: name,
      sound: sound,
      powderYell: function() {
      var yell = this.sound.toUpperCase()
      console.log(`${yell} ${yell} ${yell}!!!`)
    },
      speed: function(mph) {
        this.speed = mph
        console.log('speed:', mph)
      }
    }
    skier.powderYell()
    skier.speed("hair on fire")
    console.log(JSON.stringify(skier))
```
```javascript
// 新方案
var name = "Julia Mancuso"
    var sound = "go fast"
    const skier = {
      name,
      sound,
      powderYell() {
        let yell = this.sound.toUpperCase()
        console.log(`${yell} ${yell} ${yell}!!!`)
      },
      speed(mph) {
        this.speed = mph
        console.log('speed:', mph)
      }
    }
    skier.powderYell()
    skier.speed(350)
    console.log(JSON.stringify(skier))
```
3 扩展运算符    
（1）扩展运算符允许联合数组中的内容：
```javascript
    var peaks = ["Tallac", "Ralston", "Rose"]
    var canyons = ["Ward", "Blackwood"]
    var tahoe = [...peaks, ...canyons]
    console.log(tahoe.join(', '))
```
（2）可以创建副本，不需要改变数组原生解构
```
var peaks = ["Tallac", "Ralston", "Rose"]
var [last] = [...peaks].reverse()
console.log(last);
console.log(peaks.join(', '));
```
（3）可以用来获取数组中的其余元素
```javascript
var peaks = ["Tallac", "Ralston", "Rose", "te11", "tes12"]
var [last, ...rest] = peaks;
console.log(last);
console.log(rest.join(', '));
```
（4）可以将函数参数收集到一个数组中
```javascript
function directions(...args) {
      var [start, ...remaining] = args
      var [finish, ...stops] = remaining.reverse()
      console.log(`drive through ${args.length} towns`)
      console.log(`start in ${start}`)
      console.log(`the destination is ${finish}`)
      console.log(`stopping ${stops.length} times in between`)
    }
    directions("Truckee", "Tahoe City", "Sunnyside", "Homewood", "Tahoma")
```
（5）适用于对象
```javascript
var morning = {
  breakfast: "oatmeal",
  lunch: "peanut butter and jelly"
}
var dinner = "mac and cheese"
var backpackingMeals = {
  ...morning,
  dinner
}
console.log(backpackingMeals)
```
#### Promise对象
1 介绍    
Promise提供了一种更合理的方式处理异步行为,then函数可以通过链式调用执行某些任务，还可以使用另一个回调函数处理异常
```javascript
const getFakeMembers = count => new Promise((resolves, rejects) => {
      const api = `https://api.randomuser.me/?nat=US&results=${count}`
      const request = new XMLHttpRequest()
      request.open('GET', api)
      request.onload = () =>
           (request.status === 200) ?
            resolves(JSON.parse(request.response).results) :
            rejects(Error(request.statusText))
      request.onerror = (err) => rejects(err)
      request.send()
    })

    getFakeMembers(5).then(
      members => console.log(members),
      err => console.error(
          new Error("cannot load members from randomuser.me"))
    )
```
#### 类
1 背景介绍：    
以前javascript没有类的概念，类型定义通过函数完成，创建一个函数，通过原型在函数对象上定义方法
```javascript
    // Constructor and the prototype, the old way

    function Vacation(destination, length) {
      this.destination = destination
      this.length = length
    }

    Vacation.prototype.print = function() {
      console.log(this.destination + " will take " + this.length + " days")
    }

    var maui = new Vacation("Maui", 7)

    maui.print()
```
2 新增语法    
工作机制相同，函数即对象，继承通过原型机制实现    
```javascript
    class Vacation {

    constructor(destination, length) {
      this.destination = destination
      this.length = length
    }

    print() {
      console.log(`${this.destination} will take ${this.length} days.`)
    }

    }

    const trip = new Vacation("Santiago, Chile", 9)

    trip.print()
```
3 类的继承    
不仅可以直接继承，还可将类当做抽象类，添加新参数
```typescript
class Vacation {
      constructor(destination, length) {
        this.destination = destination
        this.length = length
      }

      print() {
        console.log(`${this.destination} will take ${this.length} days.`)
      }

    }

    class Expedition extends Vacation {
      constructor(destination, length, gear) {
        super(destination, length)
        this.gear = gear
      }

      print() {
        super.print()
        console.log(`bring your ${this.gear.join(" and your ")}`)
      }
    }

    const trip = new Expedition(
      "Mt. Whitney",
      3,
      ["sunglasses", "prayer flags", "camera"]
    )

    trip.print()
```
#### ES6模块
1 介绍    
ES6规范的javascript为模块提供了支持，javascript模块可以存储在独立的文件中，一个文件对应一个模块，
创建和导出一个模块中的数据有两种方式，用户可以从一个模块中导出一个或多个javascript对象    
2 export    
关键字export可以用来导出会在其他模块中用到的任意javascrip类型数据，其他变量将会以局部变量的形式存在。    
当值导出一个变量或导出的数据值包含一种类型时，用户可以使用export default 替换export    
3 import     
与export相对，在其他javascript文件中调用模块：    
（1）包含多个export语句的模块还可以进行对象解构    
（2）使用export default的模块还可以被导入到单个变量中：
```javascript
import freel from '/test'
```
（3）可以使用别名实现模块变量的本地化
#### CommonJS
1 介绍：    
CommonJs是所有版本的node.js都支持的模块模式。用户可以通过Babel和webpack使用，导出使用module.export;    
CommonJs不支持imporr语句，使用require进行模块导入

