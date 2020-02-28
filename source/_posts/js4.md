---
title: js基础知识（二）
date: 2018-05-24 10:42:06
tags: [web,js]
categories: 学习笔记
---

这篇先讲一下类型转换的问题
<!-- more -->
### 类型转换
 1. 显示类型转换
    - Number(mix) 转化为number
    - String(mix) 转化为string
    - Boolear(all) 转化为boolear
    - parseInt(string,radix) 
        第二个参数为指定的转换基数（2~36）；如parseInt('11',2)输出为3；在没有第二个参数时，要注意以0x开头的string会使radix默认为16进制，所以希望大家默认都加上第二个参数，防止出现意想不到的bug，并且该函数只解析整数
    - parseFloat(string) 
        该函数还可解析浮点数
    - toString(radix)
        只有number类型的toString方法可以接收radix，使得变成相应进制的string，如 var n = 17;var m = n.toString(2);此时m等于'10001'
 2. 隐式类型转换
    - isNaN()
        会把参数进行隐式类型转换
    - ++/-- ,+/-(一元正负)
    - +
    - */%
    - && || !
    - < > <= >=
    - == != (该点需注意，如果不希望有隐式类型转换的话，可以使用 === !==)
    
