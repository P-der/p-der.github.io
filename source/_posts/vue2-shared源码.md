---
title: vue2-shared源码
tags:
  - js
categories: 源码阅读笔记
date: 2022-08-08 22:56:03
---

- [掘金](https://juejin.cn/post/7129517974452437029)

### vue2-shared 源码分析
- [打包后代码地址](https://github.com/vuejs/vue/blob/dev/dist/vue.js#L14-L379)
- [源码地址 src/shared/](https://github.com/vuejs/vue/tree/dev/src/shared)
- 工具函数(util.ts)
  - emptyObject `Object.freeze({})` 冻结对象
  - isArray Array.isArray
  - isUndef 是否未定义 判断是否为`null` | `undefined`
  - isDef 是否定义 判断不是`null` && `undefined`
  - isTrue 是否为`true`
  - isFalse 是否为`false`
  - isPrimitive 是否为原始类型 判断是否为`string``number``symbol``boolean`
  - isFunction 是否为函数
  - isObject 是否为对象
  - toRawType 返回真实类型 调用`Object.prototype.toString` 获取后面的那个类型
  - isPlainObject 是否为纯对象
  - isRegExp 是否是正则
  - isValidArrayIndex 判断是否可以为数组索引，其中调用`isFinite`判断是否为有限数
  - isPromise 是否是promise
  - toString 转换为字符串
  - toNumber 转换成`number`
  - makeMap 生成一个`map`，返回一个可供查询的函数，第一个参数为字典，第二个为是否大小写敏感
  - isBuiltInTag 由`makeMap`生成的查询函数 判断是否为`slot`或`component`
  - isReservedAttribute  `makeMap('key,ref,slot,slot-scope,is')` 功能同上，检查map不同
  - remove 删除数组一个item
  - hasOwn 是否有这个`property`
  - cached 利用闭包进行缓存
  - camelize 连词符转小驼峰
  - capitalize 首字母大写
  - hyphenate 小驼峰转连词符
  - bind
    - polyfillBind bind的降级版本 针对不同参数，进行使用call和apply的取舍
    - nativeBind 原生bind
    - 分别实现两种bind 通过判断返回可用的bind
  - toArray 类数组转数组，支持以第二个参数为startIndex，返回一个新的数组
  - extend 把from的添加到to中
  - toObject 把数组每一项转换为对象，然后对这些对象做extend
  - noop 空函数
  - no 返回false的函数
  - identity 返回参数本身
  - genStaticKeys 生成静态key 把 modules 中的每个staticKeys合并并生成字符串返回
  - looseEqual 比较两个值是否相同，是宽松相等，只比值，不比引用
  - looseIndexOf 用上面方法比较数组中是否存在某个值，并返回index
  - once 执行一次的函数
  - hasChanged  与`Object.is`功能相同，不过是判断是否改变，返回相反，[mdn](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is#polyfill)
  
  ``` TypeScript
    export function hasChanged(x: unknown, y: unknown): boolean {
        if (x === y) {
            return x === 0 && 1 / x !== 1 / (y as number)  // 考虑-0、+0的情况
        } else {
            return x === x || y === y // 考虑NaN的情况
        }
    }
  ```

- 字典(constants.ts)
``` js
export const SSR_ATTR = 'data-server-rendered'
//ssr 
export const ASSET_TYPES = ['component', 'directive', 'filter'] as const
// type
export const LIFECYCLE_HOOKS = [
  'beforeCreate',
  'created',
  'beforeMount',
  'mounted',
  'beforeUpdate',
  'updated',
  'beforeDestroy',
  'destroyed',
  'activated',
  'deactivated',
  'errorCaptured',
  'serverPrefetch',
  'renderTracked',
  'renderTriggered'
] as const
// 生命周期
```

### 总结
- 函数名语义话，便于阅读与理解代码
- 函数功能固定唯一，粒子性高
- bind 的降级与 isChange 考虑了特殊情况
- 最后的vue2版本已经用ts重写了类型，类型检查是很重要的
- 阅读评论，看到一点 ES3中`undefined`是可以被赋值的。ES5之后全局的`undefined`就不能赋值了,但是局部的还是可以被赋值修改