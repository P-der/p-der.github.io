---
title: 脚本化css基础及事件
date: 2018-06-01 22:29:16
tags: [web,js,dom,event]
categories: 学习笔记
---
主要关于dom操作css的基础知识
<!-- more -->
- 读写元素css属性
 - dom.style.prop
    可读写行间样式，没有兼容性问题，碰到关键字，需要加css，复合属性必须拆解，组合单词变成小驼峰式写法，（常用的class名需要变成className查询）
- 查询计算样式
    - window.getComputedStyle(ele,null)
        计算样式只读，返回的计算样式的值都是绝对值，没有相对单位，并且ie8和ie8以下不兼容
    - ele.currentStyle
        ie独有的属性，计算样式只读，返回的计算样式的值不是经过转换的绝对值，
    - 封装兼容性方法
    ``` javascript
    function getStyle(ele,prop){
        if(window.getComputedStyle){
            return window.getComputedStyle(ele,false)[prop]
        }else{
            return ele.currentStule[prop]
        }
    }//其中getComputedStyle的第二个参数可以查询伪元素的样式
    ```
- 查找，操作样式表（这个没怎么用过）
    - document.styleSheets 该属性中存储了一个HTML文档里面的所有css样式表的集合

- 事件
    事件是交互体验的核心，通过事件，我们可以与页面进行交互，是页面的灵魂，当然内容也很重要
    - 绑定事件
        1. ele.onxxx = function (event){}
            兼容性很好，但是只能绑定一个处理程序，程序中this指向的是dom元素本身
        2. ele.addEventListener(type,fn,false);
            ie9以下不兼容，可以为一个事件绑定多个处理程序，程序中this指向的是dom元素本身
        3. ele.attachEvent('on'+type,fn)
            ie独有，也可以绑定多个处理程序，程序this指向window
        封装兼容性方法：在下面的方法中，改变了attachEvent中的this指向，而且存储了不同的处理程序，方便以后的事件解除
        ``` javascript
        function addEvent(dom,type,handle){
            if(dom.addEventListener){
                dom.addEventListener(type,handle,false);
            }else if(dom.attachEvent){
                function temp(){
                    handle.call(dom);//硬绑定this指向dom
                }
                temp.name = handle;
                dom[type] = dom[type]?dom[type].push(temp):[].push(temp);
                dom.attachEvent('on'+type,temp);
            }else{
                dom['on'+type] = handle;
            }
        }
        ```
    - 解除事件处理程序
        1. ele.onxxx = false/''/null
        2. ele.removeEventListener(type,fn,false)
        3. ele.detachEvent('on'+typs,fn)
        如果绑定匿名函数的话，无法解除；封装解除事件函数
        ``` javascript
        function removeEvent(dom,type,handle){
            if(dom.removeEnentListener){
                dom.removeEventListener(type,handle,false);
            }else if(dom.detachEvent){
                for(var i=0;i<dom[type].length;i++){
                    if(dom[type][i].name === handle){
                        dom.detachEvent(type,dom[type][i]);
                    }
                }
            }else{
                dom['on'+type] = null;
            }
        }
        ```
    - 事件处理模型---事件冒泡、捕获
        - 事件冒泡 ：
            最开始是由ie发起的，结构上嵌套关系的元素，会存在事件冒泡的功能，即同一事件，自子元素冒泡向父元素
        - 事件捕获：
            由火狐支持的，结构上嵌套关系的元素，会存在事件捕获的功能，即同一事件，自父元素捕获至子元素（事件源元素）
        - 触发顺序 先捕获，后冒泡 ie没有捕获，火狐没有冒泡，Chrome都有，另外focus、blur、change、submit、reset、select等事件不冒泡
        - 取消冒泡和阻止默认事件
            - 取消冒泡
                1. W3C标准 event.stopPropagation() ie9一下不支持
                2. ie独有的event.cancelBubble = true
                封装取消冒泡函数
                ``` javascript
                function stopBubble(event){
                    if(event.stopPropagation){
                        event.stopPropagation();
                    }else{
                        event.cancelBubble = true;
                    }
                }
                ```
            - 阻止默认事件
                1. return false;以对象属性的方式注册的事件才生效
                2. event.preventDefault();W3C标准，ie9以下不兼容
                3. event.returnValue = false;兼容ie
                默认事件包括表单提交，a标签点击跳转，右键菜单等，封装阻止默认事件的函数
                ``` javascript
                function cancelHandler(event){
                    if(event.preventDefault){
                        event.preventDefault();
                    }else if(event.returnValue){
                        event.returnValue = false;
                    }else{
                        return (function(){
                            return false;
                        }());
                    }
                }
                ```
        - 事件对象
            event||window.event 后面的用于ie
            事件源对象：1.event.target 火狐独有 2.event.srcElement ie独有的 ；而这两个Chrome都有，兼容性写法
            ``` javascript
            e = event||window.event;
            eventTarget = e.target||e.srcElement;
            ```
        - 事件委托
            利用事件冒泡，对事件源对象进行处理，比如在ul中包含大量的li，li的点击事件就可以在ul中绑定，然后利用事件冒泡，在ul的点击事件中处理，优点如下：1.性能 不需要循环所有的元素一个个绑定事件 2.灵活 当所有新的子元素时不需要重新绑定事件

