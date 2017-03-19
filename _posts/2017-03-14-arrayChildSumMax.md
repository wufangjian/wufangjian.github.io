---
layout: post
title: "求最大子数组和 [算法]"
date:   2017-03-14 16:26:16 +0800
categories: [算法]
---

> 输入一个整数数组，有正数也有负数，数组中连续一个或多个组成一个子数组，求和最大的子数组

例子：

数组：[1,2,3,-10,-5,90,-25,23,1,-1,23]

最大子数组：[90,-25,23,1,-1,23] 和为：111

# 求最大子数组和


最优解时间复杂度：`O(n)`

## 1.思路

- 1）遍历数组，对数组进行累加，同时设置[sum：当前累加值    max：当前最大累加值]

- 2）判断当前累加值，是否小于0。如果小于0，舍弃将累加sum设置为0

- 3）如果累加值大于0，判断是否大于最大累加值

- 4）返回最大累加值max

## 2.实现

```javascript
function  getChildMaxSum(arr) {
    var max = 0;
    var sum = 0;
    for(var i = 0; i < arr.length; i++){
        sum += arr[i];
        sum = sum < 0 ? 0 : sum;
        max = sum > max ? max : sum;
    }
    return max;
    
    var arr = [1,2,3,-10,-5,90,-25,23,1,-1,23];
    console.log(getChildMaxSum(arr))
}
```
