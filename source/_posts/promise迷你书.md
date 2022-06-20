---
title: Promise迷你书
date: 2022-06-19 22:38:59
tags: [js, promise]
categories: 读书笔记
---

[中文版地址](http://liubin.org/promises-book/)

## 什么是Promise

- 控制异步流程的对象
- 只能由`Pending`状态转换为`Fulfilled`和`Rejected`状态，状态不可逆
- 可以使用then方法进行接下来的处理回调

## 实战Promise

- Promise.resolve/reject  包装生成promise实例
- Promise#then 
  - then方法有两个参数，分别对应成功和失败的回调
  - return 可以进行值的传递。所传值都会由Promise.resolve进行包裹，保证传值为promise实例
- Promise#catch 
  - 相当于then的特例，只有一个失败的回调
  - ie8 兼容问题 可以使用['catch']去调用，避免保留字冲突；或者使用then方法去兼容
- Promise.all 当all中所有的promise对象都成功时调用then方法
- Promise.race 当race中存在一个结束的promise时，调用then方法

## Promise测试

- 基于mocha对promise测试
- 普通的代码采用`then`→`catch`的流程的话比较容易理解
- 将测试代码集中到`then`中处理

## Advanced

- Promise的实现类库
  
  - Polyfill
    - [jakearchibald/es6-promise](https://github.com/stefanpenner/es6-promise)
    - [yahoo/ypromise](https://github.com/YahooArchive/ypromise)
    - [getify/native-promise-only](https://github.com/getify/native-promise-only/)
  - 扩展类库
    - [kriskowal/q](https://github.com/kriskowal/q)
      - 实现了 Promises 和 Deferreds 等规范，还提供了面向Node.js的文件IO API Q-IO 等
    - [petkaantonov/bluebird](https://github.com/petkaantonov/bluebird)
      - 扩展了取消promise对象的运行，取得promise的运行进度，以及错误处理的扩展检测等非常丰富的功能，此外它在实现上还在性能问题下了很大的功夫。

- Promise.resolve 可以将thenable对象转化为promise对象，在不同的promise类库中转换可能需要thenable对象转化的操作

- 使用reject而不是throw，更安全

- Deferred和Promise
  
  - Deferred包含Promise
  
  - Deferred 具备对 Promise的状态进行操作的特权方法；即可以操控状态
    
    ```js
    function Deferred() {
        this.promise = new Promise(function (resolve, reject) {
            this._resolve = resolve;
            this._reject = reject;
        }.bind(this));
    }
    Deferred.prototype.resolve = function (value) {
        this._resolve.call(this.promise, value);
    };
    Deferred.prototype.reject = function (reason) {
        this._reject.call(this.promise, reason);
    };
    ```
  
  - Promise代表了一个对象，这个对象的状态现在还不确定，但是未来一个时间点它的状态要么变为正常值（FulFilled），要么变为异常值（Rejected）；而Deferred对象表示了一个处理还没有结束的这种事实，在它的处理结束的时候，可以通过Promise来取得处理结果。
  
  - 使用Promise.race和delay取消XHR请求
    
    ```js
    var requestMap = {};
    function createXHRPromise(URL) {
        var req = new XMLHttpRequest();
        var promise = new Promise(function (resolve, reject) {
            req.open('GET', URL, true);
            req.onreadystatechange = function () {
                if (req.readyState === XMLHttpRequest.DONE) {
                    delete requestMap[URL];
                }
            };
            req.onload = function () {
                if (req.status === 200) {
                    resolve(req.responseText);
                } else {
                    reject(new Error(req.statusText));
                }
            };
            req.onerror = function () {
                reject(new Error(req.statusText));
            };
            req.onabort = function () {
                reject(new Error('abort this req'));
            };
            req.send();
        });
        requestMap[URL] = {
            promise: promise,
            request: req
        };
        return promise;
    }
    
    function abortPromise(promise) {
        if (typeof promise === "undefined") {
            return;
        }
        var request;
        Object.keys(requestMap).some(function (URL) {
            if (requestMap[URL].promise === promise) {
                request = requestMap[URL].request;
                return true;
            }
        });
        if (request != null && request.readyState !== XMLHttpRequest.UNSENT) {
            request.abort();
        }
    }
    export { 
        createXHRPromise,
        abortPromise
    }
    ```

- done 方法(非规范，为类库实现)
  
  ```js
   Promise.prototype.done = function (onFulfilled, onRejected) {
          this.then(onFulfilled, onRejected).catch(function (error) {
              setTimeout(function () {
                  throw error;
              }, 0);
          });
      };
  ```
  
  - done 方法不会返回promise
  
  - done 方法的异常会直接抛到外面

- Promise方法链
  
  - Promisification可以转化
  
  - Promise并不是总是异步编程的最佳选择
  
  - 某些情况可以使用promise做一层封装，可以便于操作理解等

- 使用Promise进行顺序（sequence）处理

## Promises API Reference

```js
// Promise#then
promise.then(onFulfilled, onRejected);
// Promise#catch
promise.catch(onRejected);
// Promise.resolve
Promise.resolve(promise);
Promise.resolve(thenable);
Promise.resolve(object);
// Promise.reject
Promise.reject(object)
// Promise.all
Promise.all(promiseArray);
// Promise.race

```
