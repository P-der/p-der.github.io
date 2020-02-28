---
title: dom基础
date: 2018-05-28 15:00:06
tags: [web,js,dom]
categories: 学习笔记
---

这篇主要关于dom的一些基本知识，脚本化文档的内容，还包括一些window上的定时器方法

<!-- more -->

##### 什么是dom
 - 是Document Object Model的缩写，即文档对象模型。dom定义了表示和修改文档所需的对象、这些对象的行为和属性、这些对象之间的关系。是对html以及xml的标准编程接口

- 节点
    - 节点有四个属性
        1. nodeName 元素的标签名，以大写形式表示只读
        2. nodeValue text节点或comment节点的文本内容，可读写
        3. nodeType 该节点的类型，只读
        4. attributes 节点的属性机顶盒
    - 节点的类型
        1. 元素节点             1
        2. 属性节点             2
        3. 文本节点             3
        4. 注释节点             8
        5. document             9 
        6. DocumentFragment     11
     - 后面的数字是节点的 nodeType的值，通过这个值可以判断出节点的类型
     - 节点的基本操作
        - 遍历节点树
            1.parentNode 2.childNodes 3.firstChild 4.lastChild 5.nextSibing 6.previousSibing
        - 增加节点
            1.docuemnt.createElement() 2.document.createTextNode() 3.document.createComment() 4.document.createDocumentFragment()
        - 插入节点
            1.parentnode.appendChild() 2.parentnode.insertBefore(a,b) a插到b前
        - 删除节点
            parent.removeChild()
        - 替换
            parent.replaceChild(new,origin)
    - 元素节点
        元素节点比较特殊，是我们主要操作的对象，它有一些其他节点没有的方法
        - 查看元素节点
            - document代表整个文档
            - document.getElementById()  查找相应Id的元素，在IE8以下的浏览器不区分id大小写，而且也返回匹配name属性的元素
            - .getElementsByTageName() 按标签名匹配
            - .getElementsByName() 只有部分标签name可以匹配
            - .getElementsByClassName() 按类名匹配，ie8和ie8以下的ie中没有，可以同时多个class一起匹配
            - .querySelector()  css选择器，ie7和ie7之前没有，非实时，选出匹配的第一个元素
            - .querySelectorAll() css选择器，ie7和ie7之前没有，非实时，选出匹配的所有元素
        - 遍历元素节点
            1.parentElement（ie不兼容） 2.children 3.node.childElementCount === node.children.length 子节点个数（ie不兼容） 4.firstElementChild 返回第一个子元素节点（ie不兼容） 5.lastElementChild 返回最后一个子元素节点（ie不兼容） 6.nextElementSibing/previousElementSibing（ie不兼容）
        - 元素节点的一些属性
            - innerHTML 
            - innerText(火狐不兼容)/textContent(老版本ie不兼容)
        - 元素节点的一些方法
            - .setAttribute()
            - .getAttribute()
    - dom元素节点上的一些属性
        - 查看元素的尺寸 dom.offsetWidth,dom.offsetHeight
        - 查看元素的位置 dom.offsetLeft,dom.offsetTop 对于没有定位父级的元素，返回相对文档的坐标；对于有定位的父级元素，返回相对于最近的有定位的父级的坐标 dom.offsetParent 返回最近的有定位的父级
            

- window上面的一些方法
    - 定时器（这个是window的方法）
        - 创建定时器
            - var timer = setInterval() 指定时间重复执行
            - var timer = setTimerout() 指定时间执行一次
        - 删除定时器
            - clearInterval(timer)
            - clearTimerout(timer)
        - timer 的作用是为了删除的时候确定定时器
    - 查看滚动条的距离
        - window.pageXOffset/pageYOffset(ie8和ie8以下不兼容)
        - document.body/documentElement.scrollLeft/scrollTop 兼容性比较混乱，用的时候可以取两个值相加，因为这两个不同时有值，为了兼容封装兼容性的写法，代码如下
        ``` javascript
        function getScrollOffset(){
            if(window.pageXOffset){
                return {
                    x:window.pageXOffset,
                    y:window.pageYOffset
                }
            }else{
                return {
                    x:document.body.scrollLeft+document.documentElement.scrollLeft,
                    y:document.body.scrollTop+document.documentElement.scrollTop
                }
            }
        }
        ```
    - 查看视口的尺寸
        - window.innerWidth/innerHeight (ie8及ie8以下不兼容)
        - document.documentElement.clientWidth/clientHeight (标准模式下，任意浏览器都兼容)
        - document.body.clientWidth/clientHeight (适用于怪异模式下的浏览器)
        封装方法如下
        ``` javascript
        function getViewportOffset(){
            if(window.innerWidth){
                return{
                    width:window.innerWidth,
                    height:window.innerHeight
                }
            }else if(document.compatMode == 'CSS1Compat'){
                return {
                    width:document.documentElement.clientWidth,
                    height:document.documentElement.clientHeight
                }
            }else{
                return {
                    width:document.body.clientWidth,
                    height:document.body.clientHeight
                }
            }
        }
        ```
    - 查看元素的几何尺寸
        - ele.getBoundingClientRect() 兼容性很好，该方法返回一个对象，对象里面有left,top,right,bottom等属性，其中还有width,height属性，但在老版本ie中不支持，而且，这个返回值并不是实时的
    - 使滚动条滚动的方法
        - scroll(),scrollTo(),scrollBy()
        - 参数都是xy坐标，其中scrollBy方法是相对原来的位置进行的累加
