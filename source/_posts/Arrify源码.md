---
title: Arrify源码
date: 2022-08-06 22:11:52
tags: [js]
categories: 源码阅读笔记
---

- [掘金](https://juejin.cn/post/7128772727913545759)

### Arrify源码
- arrify 指的是 一个值转换为数组
- [仓库地址](https://github.com/sindresorhus/arrify)
- 功能介绍

``` js
export default function arrify(value) {
	if (value === null || value === undefined) {
		return [];
	}

	if (Array.isArray(value)) {
		return value;
	}

	if (typeof value === 'string') {
		return [value];
	}

	if (typeof value[Symbol.iterator] === 'function') {
		return [...value];
	}

	return [value];
}
```

根据代码分析处理了以下逻辑
   1. `null`和`undefined`处理为`[]`
   2. 对于数组类型直接返回
   3. 对于`string`返回`[string]`
   4. 对于存在迭代器的返回通过`...`运算符执行迭代器的返回值的集合转换为数组
   5. 其他情况返回为`[value]`
   特别的是字符串类型其实是有迭代器的，也就是满足 4 的情况，但是比较符合大家逻辑预期的其实是连续的字符串的数组返回，如果使用 4 的情况会返回拆解字符串组成的数组

### 其他
- test.js 为 js 测试
- index.d.ts 为 ts 类型定义
- index.test-d.ts 为 ts 类型定义的测试
- package 中的一些依赖
  - ava 测试 js 代码
  > AVA 是 Node.js 的测试运行程序，具有简洁的 API、详细的错误输出、新的语言特性和进程隔离，让您可以放心地进行开发
  - tsd 
  > 检查 TypeScript 类型定义
  - xo
  > JavaScript/TypeScript linter（ESLint 包装器）具有很好的默认值