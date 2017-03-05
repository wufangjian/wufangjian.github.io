---
layout: post
title: "归并排序 [算法]" 
date:   2017-03-06 00:00:16 +0800
categories: [算法]
---

> 归并是所有排序算法中时间复杂度低的算法

# 归并排序

**时间复杂度：** O(nlgn)

**基本思想：** 是一个分治策略，先进行划分，再进行合并

**归并排序算法的核心：** 先进行划分,再进行排序归并.归并两个有序的数组.即归并两个有序的数组A和B,然后就有了包含这两个新数组的数组C.


### 归并

```javascript
function merge(left, right) {
	var ret = [];
	var Llen = left.length;
	var Rlen = right.length;
	var i = 0;
	var j = 0;

	while (Llen > 0 && Rlen > 0) {
		if (left[i] > right[j]) {
			ret.push(right[j++]);
			Rlen--;
		} else if (left[i] < right[j]) {
			ret.push(left[i++]);
			Llen--;
		} else {
			ret.push(left[i++]);
			ret.push(right[j++]);
			Llen--;
			Rlen--;
		}
	}
	while (Llen > 0) {
		ret.push(left[i++]);
		Llen--;
	}
	while (Rlen > 0) {
		ret.push(right[j++]);
		Rlen--;
	}
	return ret;
}
```


### 排序

```javascript
// 排序
function mergeSort(arr) {
	if(arr.length <= 1 ) {
		return arr;
	}
	var mid = Math.floor(arr.length/2);
	var left = arr.slice(0, mid);
	var right = arr.slice(mid);
	// 归并
	return merge(mergeSort(left), mergeSort(right));
}
var arr = [9,8,7,6,5,4,3,2,1];
console.log(mergeSort(arr)); // [1,2,3,4,5,6,7,8,9];
```