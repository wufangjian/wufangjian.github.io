---
layout: post
title: "算法的一些简单应用场景 [算法]"
date:   2017-03-14 11:22:16 +0800
categories: [算法]
---

> 数组中有一个数字的出现次数超过了数组长度的一半，请找出这个数字，例如: arr = [1,2,3,4,5,3,3,3,3],3出现了5次那么3就是我们要找的数字

# 数组中出现次数超过一半的数字

最优解：时间复杂度O(n)

## 思路: 

- 1.如果数组排好序，那么中间的数字就是我们需要的 时间复杂度： `O(nlog(n))`

- 2.根据数组特点: a)保存当前出现次数最多的数字，和出现的次数  b)当前数字和前一个数字进行比较如果相等次数+1 不相等次数-1  c)如果为0，则选择当前数字，次数设置为1


## 实现：

```javascript

function MoreHalfNum(arr) {
    if(!arr || !arr[0]){
        return arr;
    }
    var times = 1;
    var ret = arr[0];
    for(var i = 1; i < arr.length; i++){
        if(times === 0){
            times = 1;
            ret = arr[i];
        }else if(ret === arr[i]){
            times++;
        } else {
            times--;
        }
    }
    return ret;
}

var num = MoreHalfNum([1,2,3,3,3,2,3]);
console.log(num); // 3
```

---

---


# 找出数组中第K大的数字

最优解：O(n)

## 思路：

受快速排序影响，找到某一个基准数字，比该基准数字大的放在数组右侧，小的放在数组左侧，大的放在数组右侧


## 实现：

```javascript

function findKnum(arr, k){
    var numK = arr[k];
    for(var i = 0 ; i < arr.length; i++){
        if(arr[i] > numK && i < k){
            var temp = arr[i];
            arr[i] = numK;
            numK = temp;
        }
        
        if(i > k && arr[i] < numK){
            var temp = arr[i];
            arr[i] = numK;
            numK = temp;
        }
    }
    
    return numK;
}

```

---

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

