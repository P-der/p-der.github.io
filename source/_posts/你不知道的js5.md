---
title: 你不知道的js(五)
date: 2022-06-14 22:20:39
tags: [js]
categories: 读书笔记
---

- [ ] 作用域与闭包

- [ ] this与对象原型

- [ ] 类型与文法

- [ ] 异步与性能

- [x] ES6与未来
  
  <!-- more -->
  
  # ES6与未来

## ES？现在与未来

- 因为兼容性问题，新语法需要转译和填补
- 对于新的语法，要去多学习😂

## 语法

- 块作用域
  
  - let、const声明
  
  - 可能存在的问题，如果在if/else中去声明函数的化，可能会存在作用域不同的情况

- rest操作符

- 默认参数

- 默认表达式

- 解构赋值
  
  - 如果对已经声明的变量进行解构，需要在()中进行

- 对象字面量扩展
  
  - 简约属性
  
  - 简约方法
  
  - getter/setter
  
  - 计算型属性名
  
  - 设置原型链 setPrototypeOf
  
  - super可用，对象可以找到原型链上进行操作

- 模板字面量
  
  - 原始字符串
    
    ```js
    function showraw(strings, ...values) {
        console.log(strings)
        console.log(strings.raw)
        console.log(values)
    }

    showraw`Hellow\nWorld`
    // ['Hellow\nWorld', raw: Array(1)]
    // ['Hellow\\nWorld']
    // []
    ```

- 箭头函数
  
  - 总是函数表达式
  
  - this指向不同，为定义时所在的上下文

- for of 循环
  
  - 可以遍历一组由迭代器生产的值

- 正则表达式扩展
  
  - unicode标志
  
  - 粘性标志y
  
  - flags 获取标志位

- 数字字面量扩展
  
  - 不同进制

- Unicode
  
  - 敏感的字符串操作 normalize 正规化操作，可用于判断长度等
  
  - 可以声明unicode变量名

- symbol
  
  - 获取作为对象属性 getOwnPropertySymbols

## 组织

- 迭代器

- generator

- 模块
  
  - 过去使用函数进行模块的划分
  
  - 现在使用es6的import就可以原生支持模块化

- 类
  
  - class
    
    - 必须与new一起使用
    
    - 不会声明提升
    
    - super是静态绑定的，不会改变
    
    - static symbol.species 指定 构造器

## 异步流程控制

## 集合

- 类型化数组 二进制类型

- Map/WeakMap/Set/WeakSet

## 新增API

- Array
  
  - Array.of 生成一个数组无逻辑负担
  
  - Array.from 转换为一个数组， 第二个参数可以转换生成数组的值
  
  - copyWithin 数组原型方法（目标， 开始， 结束）
  
  - fill 填充（value） / （value， 开始， 结束）
  
  - find/findIndex 调用函数，返回true就返回结果
  
  - entries values keys 

- Object
  
  - is 严格判断是否相等 NaN 和 NaN相等 0 与 -0不等
  
  - getOwnPropertySymbols 获取key中的symbol
  
  - setPrototypeOf 设置原型链
  
  - assign 合并对象 可枚举的和symbol可以被拷贝

- Math
  
  - 三角函数
  
  - 算数函数
  
  - 元函数
    
    - sign 返回数字的符号

- Number
  
  - 静态属性
    
    - EPSILON 最小值
    
    - MAX_SAFE_INTEGER 最大安全整数
    
    - MIN_SAFE_INTEGER 最小安全整数
  
  - isNaN 如果不是nunber 返回true 如果是，判断是否是NaN
  
  - isInteger 判断是否是整数

- String
  
  - unicode函数
    
    - fromCodePoint / codePointAt / normalize
  
  - raw
  
  - repeat 重复
  
  - startsWith/endsWith/includes

## 元编程

- 元编程：针对程序本身的行为进行编程的编程

- 函数名推断 function.name

- 元属性 new.target

- 通用Symbol
  
  - iterator
  
  - toStringTag/hasInstance toStringTag 改变[object --] 可以替换--的展示
  
  - species 构造器
  
  - toPrimitive 抽象强制转换操作
  
  - 正则表达式
    
    - match
    
    - replace
    
    - search
    
    - split
  
  - isConcatSpreadable concat的时候是否被展开
  
  - unscopables 是否可以在with中暴露

- 代理 proxy
  
  - get/set
  
  - deleteProperty
  
  - apply
  
  - construct
  
  - getOwnpropertyDescriptor
  
  - defineProperty
  
  - getPrototypOf/setPrototypeOf
  
  - preventExtensions
  
  - isExtensible
  
  - ownKeys
  
  - enumerate
  
  - has 
  
  - 有些操作无法拦截 typeof String(obj) obj+'' obj == pobj obj === pobj
  
  - 可撤销代理 
    
    ```js
    {proxy: pobj, revoke: prevoke} = Proxy.revocable(obj, handlers)
    prevoke() // 撤销代理
    ```

- Reflect 
  
  - ownKeys 可以获取所有直属的key的列表
  
  - enumrate 获取所有包括继承的非symbol、可枚举的值的迭代器

- 属性顺序 [[OwnPropertyKeys]] es6中 目前对ownKeys/getOwnPropertyNames/getOwnPropertySymbols有保证
  
  - 数字上升，数字属性名
  
  - 以创建顺序枚举直属字符串属性名
  
  - 以创建顺序枚举直属symbol属性

- 特性测试
  
  - featureTests.io

- 尾部调用优化
  
  - 可以通过蹦床
    
    ```js
    function trampoline(res) {
        while(typeof res === 'function') {
            res = res()
        }
        return res
    }
    ```
  
  - 递归展开，变成循环
  
  - try catch 捕获去判断，去自我调整

## ES6以后

- async await

- Object.observe()

- ... 扩展对象属性

- 指数操作符

- Array.includes

- SIMD

- WebAssembly(WASM)
