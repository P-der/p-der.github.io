---
title: 克隆
date: 2018-05-31 22:06:11
tags: [web,js,exercise]
categories: 练习
---
对象的克隆学习
<!-- more -->
- 浅层克隆
    ``` javascript
    function clone(origin, target){
        for(var prop in origin){
            if(origin.hasOwnProperty(prop)){
                target[prop] = origin[prop];
            }
        }
    }
    ```
    - 存在的问题：当克隆的元素为引用值的时候，由于origin和target中的元素指向同一个地址，所以当其中一个改变了元素中的值的时候，另一个也发生了改变，于是我们来学习深度克隆

- 深层克隆
    首先判断引用值可以用typeof()，然后为了区别对象和数组，可以使用以下的方法：
    1. Object.prototype.toString
    2. constructor
    3. instanceof
    ``` javascript
    function clone(origin,target){
        var toStr = Object.prototype.toString,
            compare = '[object Array]';
        for(var prop in origin){
            if(origin.hasOwnProperty(prop)){
                if(typeof(origin[prop])=='object'){
                    target[prop]=(toStr.call(origin[prop])==compare)?[]:{};
                    clone(origin[prop],target[prop])
                }else{
                    target[prop] = origin[prop];
                }
            }
        }
    }
    ```
    通过以上的代码，就可以解决浅层克隆的问题
