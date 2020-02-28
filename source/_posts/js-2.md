---
title: 原型
date: 2018-05-31 16:10:52
tags: [web,js,function]
categories: 学习笔记
---

今天来学习学习原型的知识

<!-- more -->

- 定义：原型是function对象的一个属性，它定义了构造函数制造出的对象的公共祖先。通过该构造函数产生的对象，可以继承该原型的属性和方法。原型也是对象
``` javascript
Person.prototype.lastName = 'father';// prototype原型
function Person(){
    this.name = 'person'
}
var person = new Person();//person = {name:'person'}
var person1 = new Person();//person = {name:'person'}
console.log(person.lastName)// 'father'
console.log(person1.lastName)// 'father'   person 和 person1 公用lastName
```
- 构造函数的内部实现
    ``` javascript
    function Person(){
        // var this = {
            __proto__:Person.prototype
        };
        /*...*/
        // return this;
    }
    var person = new Person();
    ```
    - constructor
        查询构造器的属性Person.prototype.constructor为Person 所以在调用person.consturctor的时候会返回Person，可以new一下这个属性执行，构造出于person相似的对象
- 原型链
    当函数使用属性或者方法的时候，会首先从自己的属性和方法中寻找，如果有的话，就执行，如果没有的话，会沿着__proto__属性进行查找，并一直重复这项操作，直到找到，或者查到最顶级（就是Object构造函数的原型，即Object.prototype，它的__proto__为null），返回undefined
- call/apply
    作用：改变this指向
    区别：传参方式不同
    ``` javascript
    fn.call(this,name,age,sex);
    fn.apply(this,[name,age,sex]);
    ```
- 继承发展史
    - 传统形式 --->原型链 效率低
    - 借用构造函数  每次调用多执行一次构造函数
    - 共享原型      如果改变原型的话会改变共享这个原型的所有构造函数    
    - 圣杯模式      比较完美的解决了共享原型的缺点
    ``` javascript
    // 圣杯模式
    //第一种写法
    var inherit = (function(){
        var F = function(){};
        return function(Origin, Target){
            F.prototype = Origin.prototype;
            Target.prototype = new F();
            Target.prototype.constructor = Target;
            Target.prototype.uber = Origin.prototype;
        }
    })()
    //第二种写法
    function inherit(Origin, Target){
        function F(){};
        F.prototype = Origin.prototype;
        Target.prototype = new F();
        Target.prototype.constructor = Target;
        Target.prototye.uber = Origin.prototype;
    }
    ```
- 命名空间
    管理变量，防止污染全局，适用于模块化开发（感觉现在也不怎么用了，有webpack等打包工具，还有ES6新语法，闭包等方法）
