---
title: js-cookie源码
date: 2022-09-08 19:25:42
tags:
  - js
categories: 源码阅读笔记
---

- [掘金](https://juejin.cn/post/7141305632220512263)

### js-cookie 源码

- [仓库地址](https://github.com/js-cookie/js-cookie)
- 功能介绍：在浏览器上操作cookie的封装
![功能介绍](https://cloud.githubusercontent.com/assets/835857/14581711/ba623018-0436-11e6-8fce-d2ccd4d379c9.gif)
- [cookie知识](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/cookie)
- 手动实现
  根据`cookie`的知识，我们可以把操作分为增删改查这几种操作，其中增删改都可以通过一个`set`去实现，所以我们主要需要完成`get`操作和`set`操作，另外我们知道一个`cookie`包括`name`、`value`、`path`等属性，其中`key`和`value`为必须的，其他不是必须的，所以我们分隔下，其他属性当作`attribute`
  1. get(name?:string)
    功能：当没有传入`name`的时候全部返回，如果传入，判断是否在`cookie`中，如果有就返回
    ``` js
        function get(name) => {
            let cookies = document?.cookie?.split(';') || []
            let cookieStore = {}
            cookies.some(cookie => {
                const [found, ...valuesArr] = cookie.split("=")
                const value = valuesArr.join("=") // 防止value存在=的情况
                cookieStore[found] = value
                return found === name
            })
            return name ? cookieStore[name] : cookieStore
        }
    ``` 
  2. set(name:string, value:string, attributes: Record<string,any>)
    功能：根据传入设置`cookie`
    ``` js
        function set(name, value, attributes) {
            let attributesOfString = Object.entries(attributes).reduce((preString, nextPreAttribute)=> {
                const [name, value] = nextPreAttribute
                if(!value) return preString
                preString += `; ${name}`
                if(value === true) return preString
                preString += `=${value.split(';')[0]}` // 防止value有;的情况
            },'') 
            
            return (document.cookie = `${name}=${value};${attributesOfString}`)
        }
    ```
  3. 其他操作：
    remove:
    ``` js
        set( name, '', {
            expires: -1
        })
    ```
  4. 基本功能到此就结束了
- 对比源码
  - 在`attribute`中添加默认对象，并提供`withAttributes`修改默认对象
  - 提供`withConverter`，提供自定义转换器，用于转换生成的cookie及查到的cookie
  - 代码上主要对比
    ``` js
        const converter = { // 默认的转换器
            read: function (value) {
                if (value[0] === '"') {
                value = value.slice(1, -1)
                }
                return value.replace(/(%[\dA-F]{2})+/gi, decodeURIComponent)
            },
            write: function (value) {
                return encodeURIComponent(value).replace(
                /%(2[346BF]|3[AC-F]|40|5[BDE]|60|7[BCD])/g,
                decodeURIComponent
                )
            }
        }
        function set() {
            //...
            attributes = assign({}, defaultAttributes, attributes) //合并默认

            if (typeof attributes.expires === 'number') { // 把传入的时间数字格式化为时间对象
                attributes.expires = new Date(Date.now() + attributes.expires * 864e5) // 864e5 为一天
            }
            if (attributes.expires) {
                attributes.expires = attributes.expires.toUTCString() // 统一格式化为标准时间字符串
            }

            name = encodeURIComponent(name) // 对 name 编码，防止不能处理的情况
                .replace(/%(2[346B]|5E|60|7C)/g, decodeURIComponent)
                .replace(/[()]/g, escape)
            //...
            return (document.cookie =
      name + '=' + converter.write(value, name) + stringifiedAttributes) // converter 为 传入的转换器，返回字符串
        }
        function get(name) {
            //...
            var found = decodeURIComponent(parts[0]) // 解码 name
            //...
        }
    ```
    整体上代码对比
    ``` javascript 
        import assign from './assign.mjs' // 合并函数
        import defaultConverter from './converter.mjs' // 获取默认转换器

        function init(converter, defaultAttributes) {
            function set (name, value, attributes) {
            //...
            }
            function get (name) {
            //...
            }
            return Object.create({
                set: set,
                get: get,
                remove: function (name, attributes) {
                    set(name, '', assign({}, attributes, {expires: -1}))
                },
                withAttributes: function(attributes) {
                    return init(this.converter, assign({}, this.attributes, attributes))
                },
                withConverter: function (converter) {
                    return init(assign({}, this.converter, converter), this.attributes)
                }
            }, {
                attributes: { value: Object.freeze(defaultAttributes) },
                converter: { value: Object.freeze(converter) }
            })
        }
        export default init(defaultConverter, { path: '/'})
    ```
    每次修改配置，都会返回一个新的操作`cookie`的对象

### 总结
- js-cookie中返回的对象中，包括了
  - `get` 获取cookie
  - `set` 设置cookie 
  - `remove` 删除cookie
  - `withAttributes` 更新默认配置
  - `withConverter` 更新默认转换器
- 通过返回的方法去操作内部的方法，利用闭包对操作进行了隔离，防止使用者对内部函数覆盖，也防止了内部函数污染全局

### 推荐
- [打包 JavaScript 库的现代化指南](https://github.com/frehner/modern-guide-to-packaging-js-library/blob/main/README-zh_CN.md)