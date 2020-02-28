---
title: 简单判断类型
date: 2018-05-31 22:31:56
tags: [web,js,exercise]
categories: 练习
---
简单判断传入值的类型
<!-- more -->
可以简单判断出来numbe string boolean function undefined object array null
``` javascript
function type(target){
    var strTemple = typeof(target);
    var toStr =  Object.prototype.toString;
    if(strTemple == 'object'){
        switch(toStr.call(target)){
            case '[object Array]':
                return 'array';
            case '[object Object]':
                return 'object';
            case '[object Number]':
                return 'object-number';
            case '[object String]':
                return 'object-string';
            case '[object Boolean]':
                return 'object-boolean';
            case '[object Null]':
                return 'null';
        }
        if(target===null){  //兼容老版本，新版本可以通过之前的switch中case返回
            return null
        }
    }else{
        return strTemple;
    }
}
```
