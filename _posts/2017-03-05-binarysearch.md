---
layout: post
title: "二分查找 [算法]" 
date:   2017-03-05 16:02:16 +0800
categories: [算法]
---

> 近期想把一些常见的算法，整理一下

## 二分查找

概念：二分查找又称折半查找

**优点：** 比较次数少，查找速度快，平均性能好

**缺点：** 要求待查表为有序表，且插入删除困难

**适用于：** `不经常变动而查找频繁的有序列表`

**过程：** 首先，假设表中元素是按升序排列，将表中间位置记录的关键字与查找关键字比较，如果两者相等，则查找成功；否则利用中间位置记录将表分成前、后两个子表，如果中间位置记录的关键字大于查找关键字，则进一步查找前一子表，否则进一步查找后一子表。重复以上过程，直到找到满足条件的记录，使查找成功，或直到子表不存在为止，此时查找不成功。


## 1.递归实现

```javascript
function binarySearch(arr, start, end, num) {
	var len = arr;
	var binary = Math.floor((start + end) / 2);
	if (num < arr[binary]) {
		return binarySearch(arr, start, binary, num);
	} else if (num > arr[binary]) {
		return binarySearch(arr, binary, end, num);
	} else {
		return arr[binary];
	}
}

var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
var flag = binarySearch(arr, 0, 9, 2);
console.log(flag);  // 2
```

## 2.非递归实现
```javascript
// 非递归
function binarySearch(arr, dist){
	var l = 0;
	var h = arr.length - 1;

	while(l <= h){
		var mid = Math.floor((l+h)/2);
		if(arr[mid] > dist){
			h = mid - 1;
		}else if(arr[mid] < dist){
			l = mid + 1;
		} else {
			return arr[mid];
		}
	}
}

var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
var number = binarySearch(arr, 1);
console.log(number); // 1
```

时间复杂度：O(lgn)