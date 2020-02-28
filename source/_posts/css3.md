---
title: css3基础知识
date: 2018-06-09 14:48:11
tags: [web,js,css]
categories: 学习笔记
---
复习一下css3一些常用的姿势
<!-- more -->

##### css3
 - css3是css2的升级版本，主流浏览器已经支持大部分的功能了，ie10后也开始全面支持css3了
 - css3前缀：在编写css3样式的时候，不同的浏览器可能需要不同的前缀，这是为了兼容各个版本的浏览器Chrome和Safari前缀为-webkit，Firefox为-moz，IE为-ms，Opera为-o

##### css3初级知识
 - border-radius:圆角
 - border-shadow:x轴偏移量 y轴偏移量 阴影模糊半径 阴影拓展半径 阴影颜色 投影方式 ---盒子阴影，可以加多个阴影，通过逗号隔开
 - text-shadow:x-offset y-offset blur(模糊程度) color
 - 背景颜色渐变gradient
    1. 线性渐变 linear-gradient([direction],color[percent],color[percent],...)
    2. 径向渐变 radial-gradient(shape r at position,color[percent],color[percent],...)
        其中shape可以为circle也可以为ellipse，position为圆心位置，r为半径，椭圆的时候该位置为x和y，为长轴和短轴
 - word-wrap:normal|bread-word;---文字边界换行
 - font-face:例子如下，其中引用多个不同格式的字体是为了兼容
 ``` css
 @font-face{
     font-family:'myFont';
     src:url('sansation_Lifht.ttf'),
        url('sansation_Lifht.eot') format('eot');
 }
 p{
     font-family:'myFont';
 }
 ```
 有一个比较全的兼容版本如下
 ``` css
 @font-face{
     font-family:'diyfont';
     src:url('diyfont.eot')/*ie9+*/
     src:url('diyfont.eot?#iefix') format('embedded-opentype'),/*ie6-ie8*/
     url('diyfont.woff') format('woff'),/*chrome,firefox*/
     url('diyfont.ttf') format('truetype'),/*chrome,firefox,opera,safari,android,ios4.2+*/
     url('diyfont.svg#fontname') format('svg');/*ios4.1-*/
 }
 ```
 - border-image:边框应用背景
 ``` css
 border-image:url(xxx.png) number stretch|repeat|round
 ```
 如上，number为截取指定图片四周的宽度为border的背景填充部分，最后一个参数为展示方式，stretch为拉伸，repeat为平铺（从中间开始），round为铺满（中间的平铺，如果大于某个极限值的时候，就会进行复制，有点像响应式布局中的随宽度进行改变，像裂变一样），在计算的时候会把引用的图片切成九宫格，最后一个就为十字部分的展开方式，number为四个角的部分的宽和高
 - background-origin:border-box|padding-box|content-box; 背景图片的起始位置，默认从padding区域开始
 - background-clip:border-box|padding-box|content-box|no-clip
    其中webkit可以做文字的clip需要配合text-fill-color属性
    ``` css
    div{
        text-fill-color:-webkit-background-clip;
        -webkit-background-clip:text;
        -webkit-text-fill-color:transparent;
    }
    ```
 - background-size:auto|长度值|百分比|cover|contain
    其中cover为*铺满*背景，超过部分截断，contain为尽量纯在一*整*张图片  --- 设置背景图片大小

##### css3选择器
 - 属性选择器
    1. E[att^='val'] 选择匹配的元素E，E定义了属性att，以val开头
    2. E[att$='val'] 选择匹配的元素E，E定义了属性att，以val结尾
    3. E[att*='val'] 选择匹配的元素E，E定义了属性att，属性的任意位置出现了val
 - 伪类选择器
    1. root 根标签选择器
    2. not 否定选择器
    3. empty 空标签选择器
    4. target 目标元素选择器，用来匹配location.hash选中的元素
    5. first-child|last-child|nth-child()|nth-last-child() 子元素选择器，如果当前位置元素不是前面所修饰的元素，无效（必须位置与元素相同）
    6. first-of-type|last-of-type|nth-of-type()|nth-last-of-type() 该种选择器与第5种的不同就是实在元素类型中位置为选中的元素
    7. only-child|only-of-type 这两个的区别于56的区别相同，是查找唯一子元素选择器
    8. enabled|disabled 可用|不可用的元素，如复选框等，可以通过伪类进行选择
    9. checked 选择框被选中的状态
    10. read-only|read-write 选中只读|非只读的元素，配合标签上的readonly使用
    这些伪类选择器在使用的时候需要在前面加':'
 - 伪元素选择器
    添加了first-letter first-line before after selection(匹配突出显示的文本)，selection中有个属性user-select:none;就是无法被选中的
 - 伪元素和伪类的区别
    伪元素效果是需要通过添加一个实际的元素才能达到的
    伪类则是选择一类，多个元素
 - 条件选择
    1. &#62; 直接子元素
    2. &#43; 后面紧挨着的兄弟节点
    3. ~ 后面的兄弟节点

##### css3进阶

###### transform形状变换
 - rotate() 旋转默认为z轴方向，有rotateX|Y|3d
 - scale() 缩放，可以接收两个参数，为x轴和y轴方向上的缩放倍数，如果没有第二个参数，默认使用第一个参数，还可以分解成scaleX()/Y/Z，还有一个scale3d() 其中就可以填写xyz的缩放倍数
 - skew() 扭曲，同scale一样，为两个参数，但是只能分解成skewX()/Y，在只有一个参数的时候y轴默认为0
 - translate() 移动，相对于自身进行移动，默认有两个参数可以分解为translateX|Y|Z，还有translate3d()
 - 特殊的是transform-origin 作用是变换圆点，默认为中央的位置，可以通过百分比，或者关键字进行修改

###### transition过渡动画
 - 它是一个复合属性，有以下几个子属性
 1. transition-property 过渡的css属性
 2. transition-duration 过渡所需时间
 3. transition-timing-function 过渡函数
 4. transition-delay 延迟时间 

###### animation动画
 - 它也是一个复合属性，主要有以下的子元素
 1. animation-name 执行动画的keyframe名，其中keyframe如下定义，如果只有0%和100%两个关键帧，可以用from to 代替
 ``` css
 @keyframes move{
  0%{...}
  10%{...}
  100%{...}   
 }
 ```
 2. animation-duration 动画执行时间
 3. animation-timing-function 过渡函数速率
 4. animation-delay 延迟时间
 5. animation-iteration-count 定义播放次数
 6. animation-direction 定义播放方向 有normal（默认值）reverse（反向播放）alternate（奇数次正向，偶数次反向）alternate-reverse（奇数次反向，偶数次正向）
 7. animation-play-state 播放状态 running（播放）paused（暂停） 
 8. animation-fill-mode 表示在结束时发生的操作 none（默认值）forwards（停留在最后关键帧的位置）backwards（开始的时候应用初始帧的位置）both（同时具有forwards和backwars效果）

###### columns多列布局
 - 主要应用在文本的多列布局，类似报纸杂志的那种布局方式
 - columns:[column-width] [column-count]，也是复合属性，可以分解，column-width为每一类的宽度，根据容器宽度自适应（最小宽度）；column-count为规定列数，不要两个一起使用
 - column-gap 设置列与列之间的宽度，没有的时候会按浏览器的font-size来定
 - column-rule 与border类似，是缝的样式，有width,style,color
 - column-span 设置多列布局元素内的子元素，标题只有1|all两个可选的值，1为在第一列，all为在所有列的上面中间

###### 弹性盒子flex布局
 > 可以观看阮一峰大佬博客中关于弹性盒子的描述，非常详细，而且有图---[阮一峰flex布局](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html?^%$)

 - 设置方式 display:flex
  - 在子元素上设置的属性
    1. flex-grow 根据设置的比例分配盒子剩余的空间
    2. flex-shrink 根据设置的比例承担超出盒子部分的大小
    3. flex-basis 伸缩基准值，和width一样，但是如果设置该属性，会覆盖width属性的值
    4. flex 前三个的复合体
    5. order 排序，从小到大排序
    6. align-self 对齐方式，允许单个子元素和其他的不同，可覆盖align-items属性，有以下的值可以填 auto|flex-start|flex-end|center|baseline|stretch auto表示继承父元素的align-items属性,如果没有父元素，等同于stretch
  - 在父元素上设置的属性
    1. flex-direction 设置主轴方向 row|row-reverse|column|column-reverse
    2. flex-wrap 设置换行 nowrap|wrap|wrap-reverse
    3. flex-flow 是前两个的复合体
    4. justify-content 元素在主轴上的对齐方式 flex-start（默认值，左对齐）flex-end（右对齐）center（居中）space-between（两端对齐，项目之间的间隔相等）space-around（项目之间两侧的间隔相等）
    5. align-items 设置在侧轴上的对齐方式 flex-start|flex-end|center|baseline|stretch
    6. align-content 设置多根轴线的对齐方式 flex-start|flex-end|center|space-between|space-around|stretch