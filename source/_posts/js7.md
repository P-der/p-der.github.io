---
title: js基础知识（五）
date: 2018-05-27 15:51:25
tags: [web,js,function]
categories: 学习笔记
---

该篇主要介绍函数的相关内容

<!-- more -->

> 在JavaScript中，函数也是一种对象，针对对象的操作方法也可以对函数使用

##### 函数的定义
``` javascript
function fn(a,b){    // 函数声明 fn不可以省略
    /* ... */
}

var foo = function fn(a,b){ // 函数定义表达式 其中fn可以省略
    /* ...*/
}

(function(){ // 立即执行函数
    /* ... */
})()
```

##### 函数的调用
 - 作为函数的调用 fn()
 - 作为方法 obj.fn()
 - 作为构造函数
 - 通过call()和apply()间接调用
 主要来看一下构造函数的这种调用方法
 ###### 构造函数
 ``` javascript
 function Fn(){
     this.age = 20;
     this.color = 'blue';
     /*...*/
 }
 var a = new Fn();
 ```
 上面就是构造函数的定义与使用方法，通过new进行调用，这点是js语言仿照类一样，通过new创造了Fn的实例，本质上如下进行实现的
 ``` javascript
 function Fn(){
     var this = {}; //其实并不是完全的空对象
     this.age = 20;
     this.color = 'blue';
     /* ... */
     return this;
 }
 ```
 如上所示实际上是在内部进行隐式的创建了一个this变量，通过里面的代码，构建完对象后把他返回，注意构造函数中如果有返回的话，返回值为对象，那么不会返回this，而是返回对象，如果返回的不是对象的话，还是返回this
 ###### 间接调用
 通过call和apply来进行调用，call和apply的区别是传参列表不同，call是一个一个参数传，apply是以数组的形式进行传参
 ``` javascript
 fn.call(this,a,b,c);
 fn.apply(this,[a,b,c]);
 ```

##### 闭包
 在这里是由于js语言的函数作用域产生的，主要表现在通过return的方式使得在函数内部的变量可以在外部进行使用，但是闭包的产生会造成内存泄露，下面来看一个比较常见的问题
 ``` javascript
 function test(){
     var arr = [];
     for(var i = 0; i < 10; i++) {
         arr[i] = function (){
             console.log(i);
         }
     }
     return arr;
 }
 var myArr = test();
 for(var j = 0; j < 10;j++) {
   myArr[j]();    //会打印出10个10
 }
 ```
 以上的问题就是由于闭包产生的，因为myArr中每一个函数中的i对应的都是test中的i，这就使得在arr中的函数被调用的时候，从test中临时取i，而i经过for循环已经变成了10，所以最后打印出来的都是10,这个问题的解决如果用ES6的话，可以直接把函数中的for循环中的var变成let，这样的话就可以完美解决，但是基础的话还可以用立即执行函数来解决
 ``` javascript
 function test(){
     var arr = [];
     for(var i = 0;i < 10;i++){
         (function(j){
             arr[j] = function(){
                 console.log(j);
             }
         })(i)
     }
     return arr;
 }
 var myArr = test();
 for(var j = 0;j < 10;j++){
     myArr[j])()
 }
 ```

##### 常用的属性方法等
 - length属性（这里指的是arguments.length，arguments是函数体中的参数列表，是类数组）
 - prototype（原型，以后会细说这个属性）
 - call()和apply()
 - bind()（该方法是ES5中的方法，用于改变函数中的this指向问题）
 - toString() 