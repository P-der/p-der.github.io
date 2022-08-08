---
title: css相关
tags: [css, 面试]
categories: 学习笔记
date: 2020-05-10 15:28:33
---
css相关知识合集
<!-- more -->

##### 盒模型
- 标准盒子模型：宽度=内容的宽度（content）+ border + padding + margin
- 低版本IE盒子模型：宽度=内容宽度（content+border+padding）+ margin
- 分别对应box-sizing属性的两个值：
    - context-box：W3C的标准盒子模型，设置元素的 height/width 属性指的是content部分的高/宽
    - border-box：IE传统盒子模型。设置元素的height/width属性指的是border + padding + content部分的高/宽

##### css选择器优先级
- !important > style 属性 > id > class > tag
- 进制为256

##### 元素水平垂直居中
- 已知宽高
    ``` css
    position: absolute;
    left: 50%;
    top: 50%;
    margin-left: -宽度的一半;
    margin-top: -高度的一半;
    ```
- 未知宽高
    ``` css
    position: absolute;
    left: 50%;
    top: 50%;
    transfrom: translateX(-50%) translateY(-50%);
    ```
- flex布局
    ``` css
    display: flex;
    justify-content: center;//水平
    align-items: center;//垂直
    ```
- 绝对定位
    ``` css
    position:absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    margin: auto;
    ```

##### 常见兼容问题
- 不同浏览器默认margin和padding不同
- IE下，可以使用获取常规属性的方法来获取自定义属性,也可以使用getAttribute()获取自定义属性；Firefox下，只能使用getAttribute()获取自定义属性。解决方法:统一通过getAttribute()获取自定义属性。
- 超链接访问过后hover样式就不出现了，被点击访问过的超链接样式不再具有hover和active了。解决方法是改变CSS属性的排列顺序:L-V-H-A ( love hate ): a:link {} a:visited {} a:hover {} a:active {}

##### display:none与visibility：hidden的区别？
- display：none 不显示对应的元素，在文档布局中不再分配空间（回流+重绘）
- visibility：hidden 隐藏对应元素，在文档布局中仍保留原来的空间（重绘）

##### css引入
- 内部引入
    - 行内引入：在标签内添加style属性
    - style标签引入
- 外部引入
    - link标签引入
    - style标签中@import引入
    ``` css
    <link type="text/css" rel="styleSheet"  href="CSS文件路径" />
    <style type="text/css">
      @import url("css文件路径");
    </style>
    ```
- link与@import区别：
    - 页面被加载时，link会同时被加载，link 样式的权重高于@import
    - @import引用的css会等到页面加载结束后加载，@import只有IE5以上才能识别

##### margin塌陷
- ...