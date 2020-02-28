---
title: js基础知识（四）
date: 2018-05-25 23:47:05
tags: [web,js,array]
categories: 学习笔记
---

这篇主要是ES5中的一些array方法

<!-- more -->

- forEach()
    遍历数组并使每一个元素都调用指定的函数,需要注意的是无法终止遍历，只能通过try才能停止
 ``` javascript
 arr.forEach(function(item,index,array){
     /* ... */
 })
 //源码仿写
 Array.prototype.forEach = function (func) {
    var arr = this;
    for(var i=0;i<arr.length;i++){
        func(arr[i],i,arr);
    }
 }
 ```
- map()
    该方法将每个元素使用指定的函数，并返回一个新数组，不改变原数组
 ``` JavaScript
 var b = arr.map(function(item,index,array){
     /* .... */;
     return /*处理完的某个值，由这些值组成新的数组*/
 })
 //源码仿写
 Array.prototype.map = function (func) {
    var arr = this;
    var newArr = [];
    for(var i=0;i<arr.length;i++){
        newArr.push( func(arr[i],i,arr) );
    }
    return newArr;
 }
 ```
- filter()
    该方法就是它的意思，过滤，把每一个元素进行判断，返回true或false，最后生成一个新的数组
 ``` JavaScript
 arr.filter(function (item,index,arr){
    /* ... */ 
    return ;//返回true，保留，反之，删除
 })//最后会返回一个新数组
 //源码仿写
 Array.prototype.forEach = function (func) {
    var arr = this;
    var newArr = [];

    for(var i=0;i<arr.length;i++){
        if(func(arr[i],i,arr)){
            newArr.push(arr[i]);
        }
    }
    return newArr;
 }
 ```
- every()和some()
    这两个方法，一个是判断每一个，一个是判断存在一个就行，也就是说every针对所有，必须所有都满足才为true，而some是存在一个就行就返回true；并且，some在遇到第一个true时就结束遍历；every在遇到第一个false时结束遍历
 ``` javascript
 arr.some(function (item,index,arr){
    /* ... */ 
    return ;
 })//最后会返回true或false;
 arr.every(function (item,index,arr){
    /* ... */ 
    return ;
 })//最后会返回true或false;
 //源码仿写
 Array.prototype.some = function (func) {
    var arr = this;
    for(var i=0;i<arr.length;i++){
        if(func(arr[i],i,arr)){
            return true;
        }
    }
    return false;
 }
 Array.prototype.some = function (func) {
    var arr = this;
    for(var i=0;i<arr.length;i++){
        if(!func(arr[i],i,arr)){
            return false;
        }
    }
    return true;
 }
 ``` 
- reduce()和reduceRight()
    使用指定的函数将数组元素进行组合，生成单一值
 ``` javascript
 var a = [1,2,3,4];
 var sum = a.reduce(function(x,y){return x+y},0); // 求和 
 ```
    运行过程是把第二个参数和数组中的第一个元素传入函数，分别对应xy，并把返回值作为下一个的x，y为下一个元素，如果没有第二个参数的话，会把第一个元素当做x，第二个元素为y，进行以上的运算
    reduce()和reduceRight()的区别是，以上的运行过程，reduceRight则完全相反，第一个y为最后一个元素
- indexOf()和lastIndexOf()
    indexOf 从前向后查找数组中是否含有给定的元素，返回找到的第一个元素的索引值，没有的话返回-1；lastIndexOf与indexOf的查找顺序相反，需要注意的是字符串也有该方法，并且用法一样