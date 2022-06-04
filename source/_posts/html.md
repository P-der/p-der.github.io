---
title: html相关
date: 2020-05-10 14:11:32
tags: [html,面试]
categories: 学习笔记
---
html相关知识合集
<!-- more -->

##### DOCTYPE
- Doctype 告诉浏览器使用了哪个版本的HTML / XHTML标准以及如何呈现页面。浏览器有两种html渲染模式，分别是标准模式和混杂模式，如果使用DOCTYPE则使用标准模式，采用最新的标准，否者使用混杂模式向前兼容（主要不同如盒模型）
    ###### 使用方法
    ```
    <!DOCTYPE html>
    ```

##### 行元素、块元素和行级块元素
- 行元素
	1. 不能设置宽和高，根据自身内容决定占据空间大小
	2. 包括：span a em stong del
- 块元素
    1. 可以设置宽和高，独自占据一整行
    2. 包括：div p ol li ul form address
- 行级块元素
    1. 可以设置宽和高，根据自身内容决定占据空间大小
    2. 包括：input img

##### src与href区别
- src 
    1. src是source的缩写，指向外部资源的位置，指向的内容将会嵌入到文档中当前标签所在位置。
- href
    1. href是Hypertext Reference的缩写，指向网络资源所在位置，建立和当前元素（锚点）或当前文档（链接）之间的链属接。

##### 其他
- p标签的子元素不能是div