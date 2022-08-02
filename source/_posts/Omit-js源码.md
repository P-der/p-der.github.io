---
title: Omit.js源码
date: 2022-08-02 21:56:57
tags: [js]
categories: 源码阅读笔记
---


- [掘金](https://juejin.cn/post/7127275216807395341)

### Omit.js 源码分析
- [仓库地址](https://github.com/benjycui/omit.js)
- 源码路径 omit.js/src/index.js
- 功能介绍

``` js
var omit = require('omit.js');
omit({ name: 'Benjy', age: 18 }, [ 'name' ]); // => { age: 18 }
```
   
   如上所示，通过第二个参数剔除，不需要的 key
- 源码分析

``` js
function omit(obj, fields) {
  const shallowCopy = Object.assign({}, obj);
  for (let i = 0; i < fields.length; i += 1) {
    const key = fields[i];
    delete shallowCopy[key];
  }
  return shallowCopy;
}

export default omit;
```

1. 通过 Object.assign 创建出一个浅拷贝的对象
2. 循环 fields，删除拷贝对象中对应的 key
3. 返回拷贝值

- 对比 [lodash/omit](https://github.com/lodash/lodash/blob/es/omit.js)

1. lodash 考虑了 深拷贝的情况（fields 中是否有 数组的子项）
2. lodash 考虑了边界情况，如 obj 是否为 null；fields 是否为一个类数组；fields 中是否有特殊的 key（__proto__）
3. lodash 没有使用 es6 的方法，兼容性会更好些

### npm 包所包含的内容
 - /tests jest测试
 - [father](https://github.com/umijs/father)
    >  Library toolkit based on rollup, docz, storybook, jest, prettier and eslint.
    包括了库的打包、输出文档、测试及格式化
- LICENSE 证书







