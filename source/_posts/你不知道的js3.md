---
title: 你不知道的js(三)
date: 2022-06-06 17:54:55
tags: [js]
categories: 读书笔记

---

- [ ] 作用域与闭包

- [ ] this与对象原型

- [x] 类型与文法

- [ ] 异步与性能

- [ ] ES6与未来
  
  <!-- more -->
  
  # 类型与文法

## 类型

- 内建类型 `null` `undefined` `boolean` `number` `string` `object` `symbol` `bigInt`
- 对于未定义的变量判断，使用typeof的安全防卫机制（防止一个错误）是有用的

## 值

- array
  
  - 不建议使用string作为key

- 类array
  
  - Array.prototype.slice.call 
  - Array.from

- string
  
  - 可以通过Array.prototype.join.call去调用数组方法处理字符串

- number
  
  - 小数值判断：Number.EPSILON 最小的一个容差，如果小于就认为是相等的
  
  - 判断整数：Number.isInteger
  
  - undefined 在非严格情况下可以作为变量名进行赋值操作，可以通过void 0 获取一个安全的undefined
  
  - NaN 唯一一个不等于本身的数
    
    - 全局的isNaN判断是转换成number类型进行判断的
    - Number.isNaN 可以进行判断
  
  - 无穷 可以变成无穷，但是不可以反操作， Infinity/ Infinity 为NaN
  
  - 零 +0 -0 0
    
    - == 或=== 时都是相等的
  
  - 特殊等价 Object.is 忽略前面的，符合一般的逻辑
    
    ```js
    Object.is = function(v1, v2) {
        if(v1 === 0 && v2 === 0) { // 判断-1的情况
            return 1/v1 === 1/v2
        }
        if(v1 !== v1) { // 判断NaN的情况
            return v2 !== v2
        }
        return v1 === v2 // 其他情况
    }
    ```

- 值与引用
  
  - 需要注意是否使用的引用，需要注意出现的更改影响范围

## 原生类型（内建类型）

- 也就是js内常用的构建函数

- typeof 结果通常为object 需要通过Object.prototype.toString.call 的返回值去判断具体类型

- 包装类 需要注意故意包装的类型是object 如果判断结果一定是true

- 开箱 valueof 

- Array 需要注意各种生成空值数组的情况，不建议使用

- Function.prototype 是一个函数 RegExp.prototype 是一个正则表达式 Array.prototype 是一个数组

## 强制转换

- 抽象值操作
  - ToString null => 'null' undefined => 'undefined' true => 'true'
    - JSON.stringify 遇到undefined function symbol 会忽略 如果在array中会转换为null 如果遇到循环引用会报错， 内部实际调用了toJSON的方法，string number boolean null 调用的结果与tostring相同
  - ToNumber
    - string类型是希望转换为数字，如果不可以就为NaN
    - 对象会 ToPrimitive操作 执行valueof（如果有，并且返回为基本类型） 然后去执行toString，都没有就会报错，然后toNumber
  - ToBoolean
  - 转为false的值为：`undefined` `null` `+0/-0/NaN` `false` `''`, 其余都为true
- 明确的类型转换
  - +转换字符串为数字 ～转换-1为0 ，～x = -(x+1) ～～截取整数
  - parseInt 建议一直写第二个参数为10，防止判断为其他进制, 如果传入的不是string类型的参数，则会toString 然后再去执行操作
- 隐含的类型转换
  - string <--> number
    - +求和时： 如果其中有一个为string 则变成string相加，其他类型会ToPrimitive然后相加
  - symbol
    - 可以通过String转换为字符串，但是不可以用+''去转换
  - == 与 === ==允许类型转换 === 不允许
    - 类型相同，直接比较，如果是引用类型，比较引用是否相同
    - string与number比较，toNumber后进行比较
    - boolean与其他类型比较，toNumber（boolean）然后进行比较
    - null 与 undefine 为 true 和其他值比较为false
    - object与其他比较 toPrimitive后进行比较
  - 比较大小时，如果有一个为number，另一个会 会toPriitive 然后执行toNumber， 如果都为字符串，则按字符比较

## 文法

- 语句与表达式
  - js中 语句为句子 表达式为短语
- 语句完成值，就是语句结束在控制台上可以看到的值
- [] + {} 与 {} + []
  - [] + {} => '' + '[object Object]'  => '[object Object]' 其中 {} 为 空对象
  - {} + [] => + []  => + '' => 0 其中{} 为一个空的代码块
- else if  其实并不存在 else{ if }
- && 比 ||优先级高
- 加不加分号
  - 建议是，尽可能的加分号
- 暂时性死区
  - typeof 也是一样会报错
- 函数参数，不要即用参数变量又用argument的值，会产生歧义
  - 非严格模式下，如果传入参数，参数变量修改会映射到argument上，否则不会
  - 严格模式下，参数变量修改不会影响argument
- try...finally 会执行try，然后执行finally 最后如果是函数返回，才会继续执行
