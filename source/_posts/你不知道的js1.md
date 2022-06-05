---
title: 你不知道的js(一)
date: 2022-06-04 17:51:47
tags: [js]
categories: 读书笔记

---

- [x] 作用域与闭包

- [ ] this与对象原型

- [ ] 类型与文法

- [ ] 异步与性能

- [ ] ES6与未来
  
  <!-- more -->
  
  # 作用域与闭包
  
  ## 什么是作用域

- 编译器，是先进行编译，然后去运行的

- 作用域是通过标识符号名称查询变量的一组规则

- LHS和RHS 个人理解RHS是需要知道变量的值 LHS是需要知道变量的容器
  
  ## 词法作用域

- 词法分析时进行定义的，意味着在你定义一个函数的时候就决定了它的作用域

- 欺骗词法作用域
  
  - evel  执行字符串，就好像原来就在这个位置上一样
  
  ```js
  function foo(str) {
      evel(str)
      console.log(a,b)
  }
  var b = 2
  foo('var b = 3', 1) // 1 3
  ```
  
  - with 在运行时把传入对象作为作用域去使用，需要注意的是，如果对象没有这个，就会导致全局的变量声明
  - 上面两者的缺点是，无法提前知道内容是什么，导致引擎做的优化全部失效，会有性能问题
  
  ## 函数与块作用域

- 函数作用域，每声明一个函数，每一个函数就会生成一个自己的作用域

- 在函数内定义函数，去隐藏作用域
  
  - 避免冲突 
  
  - 全局名称空间
  
  - 模块管理

- 函数作为作用域 利用立即执行函数去隐藏

- 块作为作用域 
  
  - with
  
  - try/catch 在catch子句上拥有块作用域
  
  - let/const 劫持{}生成块作用域
  
  - 使用块作用域可以更好的做垃圾回收
  
  - let 解决 for 循环
  
  ## 提升

- 函数优先 重复函数声明，后面的会覆盖前面的
  
  ## 作用域闭包

- 闭包：函数能够记住并访问它的词法作用域，即使这个函数在它的词法作用域之外执行时

- 模块
  
  - 利用闭包可以实现一个简单的模块加载
    
    ```js
    var MyModules = (function Manager(){
        var modules = {};
        function define(name, deps, impl) {
            for(var i = 0; i< deps.length; i++) {
                deps[i] = impl.apply(impl, deps)
            }
            modules[name] = impl.apply(impl, deps)
        }
        
        function get(name) {
            return modules[name]
        }
        
        return {
            define,
            get
        }
    }()    
    ```
  
  - es6模块