---
title: 快排学习
date: 2018-05-21 22:49:29
tags: [web,js]
categories: 学习笔记
---

> 前段时间逛掘金的时候，看到了掘金上的热门，关于快速排序的，虽然那篇文章很有争议，我还是看了看，发现了自己的一些不足，于是学习了一下快速排序

<!-- more -->

### 快速排序介绍

    算法导论上是这样写的
> 对一个典型子数组A[p..r]排序的分治过程的三个步骤：
 ###### 分解
  数组A[p..r]被划分成两个（可能空）子数组A[p..q-1]和A[q+1..r]，使得A[p..q-1]中的每个元素都小于等于A(q)，而且，小于等于A[q+1..r]中的元素。小标q也在这个划分过程中进行计算。
 ###### 解决
  通过递归调用快速排序，对子数组A[p..q-1]和A[q+1..r]排序。
 ###### 合并
  因为两个子数组是就地排序的，将它们的合并不需要操作：整个数组A[p..r]已排序。

    下面的过程实现快速排序：
    ```
    QUICKSORT(A,p,r)
     if<r
      then q <-- PARTITION(A,p,r)
       QUICKSORT(A,p,q-1)
       QUICKSORT(A,q+1,r)   
    ```
    为排序一个完整的数组A，最初的调用是QUICKSORT(A,1,length[A])。
###### 数组划分
    快速排序算法的关键是PARTITION过程，它对子数组A[p..r]进行就地重排：
    ```
    PARTITION(A,p,r)
     x <-- A[r]
     i <-- p-1
     for j <-- p to r-1
      do if A[j]<=x
       then i <-- i+1
        exchange A[i]<-->A[j]
     exchange A[i+1]<-->A[r]
     return i+1
    ```
###### 图解
![快速排序图解（无耻抠图）](https://images2015.cnblogs.com/blog/634705/201509/634705-20150921205939850-1052509861.png)

#### JavaScript代码实现

``` JavaScript
function quicksort(arr,l,r){
    if(l<r){
        var q = partition(arr,l,r);
        quicksort(arr,l,q-1);
        quicksort(arr,q+1,r);
    }
}
function partition(arr,l,r){
    var x = arr[r];
    var i = l-1;
    for(var j = l;j<r;j++){
        if(arr[j]<=x){
            i++;
            var temp = arr[j];
            arr[j] = arr[i];
            arr[i] = temp;
        }
    }
    arr[r] = arr[i+1];
    arr[i+1] = x;
    return i+1;
}
```  
以上就是快排的基本实现，仿照算法导论进行的编写，嘿嘿嘿  