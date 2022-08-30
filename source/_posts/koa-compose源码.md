---
title: koa-compose源码
tags:
  - js
categories: 源码阅读笔记
date: 2022-08-30 19:40:20
---

- [掘金](https://juejin.cn/post/7137632272970153991/)

### koa-compose 源码

- [仓库地址](https://github.com/lxchuan12/koa-compose-analysis)

- 功能介绍：洋葱模型式调用中间件函数 [官网介绍](https://github.com/koajs/koa/blob/master/docs/guide.md#writing-middleware)
![功能介绍图](/images/koa-compose.awebp)


- 源码分析
``` js
function compose (middleware) {
  // 参数检测
  if (!Array.isArray(middleware)) throw new TypeError('Middleware stack must be an array!')
  for (const fn of middleware) {
    if (typeof fn !== 'function') throw new TypeError('Middleware must be composed of functions!')
  }

  /**
   * @param {Object} context
   * @return {Promise}
   * @api public
   */
  return function (context, next) { // context 上下文，用于共享状态类似store， next 自定义到中心后的处理函数
    // last called middleware #
    let index = -1 // 初始值  
    return dispatch(0) // 初始值从 0 开始
    function dispatch (i) {
      if (i <= index) return Promise.reject(new Error('next() called multiple times')) // 防止重复调用
      index = i // index 获取当前 middleware 调用指针
      let fn = middleware[i] // 获取获取当前 middleware 函数
      if (i === middleware.length) fn = next // 如果到了最后一个，赋值为 compose(middleware)(context, next) 中 next 的值，也就是说可以定义结束时的自定义next
      if (!fn) return Promise.resolve() // 如果 fn 不存在，走到这里证明没有传入next 就可以结束了
      try {
        return Promise.resolve(fn(context, dispatch.bind(null, i + 1))) // promise调用， 函数传入 context 和 第二个 dispath，在调用 next 的时候就是第二个的调用，依次类推
      } catch (err) {
        return Promise.reject(err) // 错误结束
      }
    }
  }
}
```

### 总结
- 学习了`koa-compose`的实现，通过`Promise.resolve`第二个参数进行递归调用`dispatch`执行后面的中间件函数
- 学习使用`jest`分步调试代码
- 设计模式-责任链模式： 是一种处理请求的模式，它让多个处理器都有机会处理该请求，直到其中某个处理成功为止；这里的中间件也就是多个的处理器，只需要去关心自己要处理的内容，结合洋葱模型，`koa`使责任链中先经过请求，然后响应，通过中间件函数中的 `next`去做，响应与请求的分割，通过 `context` 共享数据