---
title: 你不知道的js(二)
date: 2022-06-05 16:56:06
tags: [js]
categories: 读书笔记
---

- [ ] 作用域与闭包

- [x] this与对象原型

- [ ] 类型与文法

- [ ] 异步与性能

- [ ] ES6与未来
  
  <!-- more -->

# this与对象原型

## this是什么

- this是运行时绑定的，依赖于函数调用的上下文条件，与定义无关，与函数调用的方式有关，执行环境的活动记录中存在this的引用

## this豁然开朗

- 如何判定this
  - 函数是否是通过new调用，如果是，this就是新构建的对象
  - call/apply绑定，this为绑定的对象
  - 对象调用，this为调用的对象
  - 默认绑定，如果是严格模式为undefined，否则为global对象
- 绑定的特例
  - 如果null/undefined作为call/apply/bind的this，那么会忽略掉
  - (p.foo = o.foo)() 如此间接调用会采用默认调用
  - 词法this 箭头函数，只与定义时候的环境有关

## 对象

- 属性名为字符串或Symbol
- 对象的拷贝
  - 浅拷贝 复制对于value的引用
  - 深拷贝 复制value引用的内容
- 属性描述符
  - value 值
  - writable 可写性
  - enumerable 可枚举性
  - configurable 可配置性
- 对象的不可变性实现
  - 对象常量 writable+configurable = false 
  - 防止扩展 Object.preventExtensions
  - 封印Seal configurable = false +防止扩展 Object.seal
  - 冻结Freeze  封印+对象常量
- 对象的存在性
  - in  是否存在于对象及原型链中
  - hasOwnProperty 是否存在于对象中
  - 枚举 对象的属性在迭代时是否会包含在内
    - 区分 propertyIsEnumerable
    - Object.keys 返回所有可枚举属性数组 针对当前对象
    - Object.getOwnPropertyNames 返回所有属性的数组包括不可枚举的类型 针对当前对象
- 迭代
  - 定义对象的Symbol.iterator可以使对象可以调用迭代的方法如for of

## 混合类的对象

- js中不存在类，类是一种设计模式，在js中可以去模拟类，会存在拷贝的问题

## 原型

- 设置与遮蔽属性
  - 1 如果在原型链中并且可以writable 则会在原对象中创建赋值
  - 2 如果writable：false 则忽略赋值操作
  - 3 如果在原型链中找到并且是setter 则调用setter，不会在原对象中进行操作
  - 如果遇到上面2/3的情况，则需要使用Object.defineProperty 去添加属性
- fn.isPrototypeOf(a) 判断a是否是fn的实例 getPrototypeOf() 获取原型链

## 行为委托

- 实际上就是利用原型链进行方法的继承管理，是一种没有类的抽象，基于原型链的行为委托