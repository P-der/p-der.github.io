---
title: yocto-queue源码
tags:
  - js
categories: 源码阅读笔记
date: 2022-08-09 16:44:21
---

- [掘金](https://juejin.cn/post/7129753511926956046)

### yocto-queue 源码分析
- [仓库地址](https://github.com/sindresorhus/yocto-queue)
- 功能介绍
  > 这是一个队列的实现，并通过迭代器实现了用`...`运算符转换为数组
- 源码分析
  - Node 类：用于创建队列中的一个节点
  ``` js
  
    class Node {
        value;
        next;

        constructor(value) {
            this.value = value;
        }
    }
  
  ```
  - Queue 类：队列核心
  
  ``` js
    export default class Queue {
        // 私有变量
        #head; // 保存队列头
        #tail; // 保存队列尾
        #size; // 保存队列长度

        constructor() {
            this.clear(); // 初始化
        }

        enqueue(value) { // 入列
            const node = new Node(value); // 创建节点

            // 保存状态
            if (this.#head) {
                this.#tail.next = node;
                this.#tail = node;
            } else {
                this.#head = node;
                this.#tail = node;
            }

            this.#size++; //更新长度
        }

        dequeue() { // 出列
            const current = this.#head; // 获取对头
            if (!current) { // 是否存在
                return;
            }

            this.#head = this.#head.next;  // 移动头
            this.#size--; // 更新长度
            return current.value; // 返回对头值
        }

        clear() { // 私有变量重新初始化
            this.#head = undefined; 
            this.#tail = undefined;
            this.#size = 0;
        }

        get size() { // 返回内部长度变量
            return this.#size;
        }

        * [Symbol.iterator]() { // 迭代器，用于转换为数组
            let current = this.#head; // 获取对头

            while (current) { // 循环队列
                yield current.value; // 使用yield输出节点值
                current = current.next;
            }
        }
    }
  ```

### 总结
- 一个简单的单向链表队列实现：队列可以理解为生活中的排队，比如地铁站的排队，先到的先进
- 使用了迭代器便于操作者的转换使用
- 对比数组：
  - 数组存储是连续的，队列链表是通过指针的
  - 数组的删除添加更灵活(O(n))，队列链表只能在后面插入，前面移出(O(1))
  - 数组的查找简单(O(1))，队列链表需要循环(O(n))