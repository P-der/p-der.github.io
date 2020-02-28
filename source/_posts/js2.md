---
title: js事件——点击事件与失焦事件冲突解决
date: 2018-5-9 22:28:37
tags: [web,js]
categories: 学习笔记
---

> 前端菜鸟，哈哈哈，在练习的过程中，遇到了事件冲突问题，并通过薅头发的方法得以解决，希望通过和大佬们进行讨论，学习一下是否有更好的解决方法

<!-- more -->
### 问题

  如豆瓣搜索框，在搜索时，会显示一个下拉列表，这个时候如果点击搜索框外面，会触发blur事件，下拉列表隐藏；如果点击下拉列表，则会跳转到相应的页面，在练习过程中，这个环节出了一些问题，我的部分代码如下
  
``` javascript
$('input').on('blur',function(e){
    $('list').css({display:'none'})
})
```

结果在点击下拉列表的时候无法进行跳转（在下拉列表中有a标签包裹，所以利用默认事件进行跳转页面）

### 分析问题

通过分析可以知道整体操作只与click事件和blur事件有关，故此判断触发顺序

``` javascript
$('input').on('blur',function(e){
    console.log(1)
})
$('a').on('click',function(e){
    console.log(2)
})
// 输出结果
//1
//2
```

由此可知，先触发了blur事件，然后触发了click事件，所以，在blur事件中使list的display：none；就会在触发click事件之前把a隐藏，这样的话，就无法点击a标签，进而无法触发点击事件

### 解决方法

利用事件冒泡，首先无论如何blur事件一定先触发，所以隐藏list不能在blur事件中处理，那么应该在click事件中处理，又因为不能单单在a标签中处理（否则点击页面其他部分触发blur事件时就无法使list隐藏），所以利用事件冒泡，在body上进行隐藏处理，代码如下:

``` javascript
$('body').on('click',function(e){
    if(e.target.tagName!=='INPUT'){
        $('list').css({display:'none'});
    }
})
```

通过判断事件源如果不是点击输入框，就可以判断为点击到了外部，此时会默认触发blur事件，而且会使list隐藏；如果点击的是输入框，则不隐藏list

掘金 P-der，希望得到大佬指教
