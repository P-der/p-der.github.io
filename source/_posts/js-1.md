---
title: 闭包
date: 2018-05-31 10:54:31
tags: [web,js,function]
categories: 学习笔记
---

在学习闭包前，我们需要一些知识，这些知识会方便我们学习闭包

<!-- more -->

- js运行三部曲
 1. 语法分析（自己写错了代码等）
 2. 预编译（重点）
 3. 解释执行

##### 预编译
 - 预编译前奏
    1. 暗示全局变量：如果变量未经声明就赋值，此变量为全局对象所有（严格模式报错），需要注意的是 var a = b = 123;这种样子的，其中变量b并没有被声明，所以b也是全局对象所有的
    2. 一切声明的全局变量都是window的属性，window上的属性在使用时，不需要写window；
 - 预编译四部曲
    1. 创建AO对象（执行期上下文）
    2. 找函数里面的形参和变量声明，并变成AO的属性名，值为undefined
    3. 实参与形参统一
    4. 找到函数声明，变成AO的属性名，并把函数体当做值赋给它
 - 执行的时候就变量声明和函数定义等，已经完成了提升，这是声明语句就会被忽略掉，进而去执行语句，这时就是解释执行阶段

##### 作用域
 - 运行期上下文：当函数执行时，会创建一个称谓执行期上下文的内部对象。一个执行期上下文定义了一个函数执行时的环境，函数每次执行时的执行上下文都是独一无二的，所以多次电泳一个函数会导致创建多个执行上下文，函数每次执行时都会把新生成的执行期上下文，填充到作用域链的最顶端，当函数执行完毕，它产生的执行上下文被销毁
 ``` javascript
 //eg
 function a(){
     function b(){
         var b = 234;
     }
     var a = 123;
     b();
 }

 var glob = 100;
 a();
 /*
 函数创建的时候，继承当前所在的作用链，执行的时候，把预编译产生的activation object对象放到执行期上下文的顶部
 1. 当a创建时 
 a.[[scope]] --> scope chain
                  0         --> global object       {this:window,window:{},document:{},a:fn,glob:100}
 2. 当a函数执行的时候
 a.[[scope]] -->scope chain
                  0         --> activation object   {this: window,arguments:[],a:123,b:fn}
                  1         --> global object       {this:window,window:{},document:{},a:fn,glob:100}                      
 3. 当b创建时，b会继承a的作用域链
 b.[[scope]] -->scope chain
                  0         --> activation object   {this: window,arguments:[],a:123,b:fn}
                  1         --> global object       {this:window,window:{},document:{},a:fn,glob:100}         
 4. 当b函数执行的时候
 b.[[scope]] -->scope chain
                  0         --> activation object   {this:window,argument:[],b:234}  
                  1         --> activation object   {this: window,arguments:[],a:123,b:fn}               
                  2         --> global object       {this:window,window:{},document:{},a:fn,glob:100}    
 */
 ```
 - 闭包
  闭包就是源于上面作用域产生的，例如上面例子中的b函数不执行，而是直接返回，这样的话，首先a函数执行结束，掐断自己的作用域链指向，而在返回的b中，仍然还保存a的activation object ，此时就产生了闭包，闭包造成了内存泄露，但是却产生了一些非常好的特性，1.实现共有变量2.可以做缓存3.可以实现封装，属性私有化4.模块化开发，防止污染全局变量