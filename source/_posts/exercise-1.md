---
title: 数组去重
date: 2018-05-31 22:54:30
tags: [web,js,exercise]
categories: 练习
---
数组去重练习
<!-- more-->
在原型链上进行编程
``` javascript
Array.prototype.unique = function(){
    var obj = {};
    var arr = [];
    var len = this.length;
    for(var i = 0; i < len; i++){
        if(!obj[this[i]]){
            obj[this[i]]=true;
            arr.push(this[i]);
        }
    }
    return arr;
}
```

