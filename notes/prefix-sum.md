---
title: Prefix Sum
tags:
  - algorithms
emoji: 💻
link:
---

## Prefix Sum

### Code
```js
function calcPrefixSum(arr) {
	const prefixSumArr = new Array(arr.length);
	prefixSumArr[0] = arr[0];
	for (let i = 1; i < arr.length; i++) {
		prefixSumArr[i] = prefixSumArr[i - 1] + arr[i];
	}
	return prefixSumArr;
}

function prefixSumRed(arr) {
	return arr.reduce(
		(acc, curVal, index) => acc.concat(curVal + (acc[index - 1] || 0)),
		[]
	);
}
```

### Explanation

To calculate a sum of elements in an array, you could loop through the array and calc the result by adding numbers to a sum var.
```js
let sum = 0;
arr.forEach(el => sum += el);
```
That would give you a time complexity of O(n).

However, to calculate the sum again of a subarray portion, you would need to perform the same operation with time complexity of O(m), m being length of the subarray. The problem arises if you need to calculate the various sums of the subarrays in a row. To perform m number of queries of n size array, time complexity rises to O(n*m).

Prefix sum algorithm takes O(n) time to perform m number of queries to find range sum on n size array in a given range (contiguous segment of array). To calculate the sum, we take current element plus the previous element.

So input arr: [2, 4, 7, -5, 3]. Prefix: [2, 6, 13, 8, 11]

Then, to select the sum of the range from first element, you just need to look at the last position (ex. [0, 4 - the sum would be in position 4]).

To calculate the sum in a subarray, you need to perform a calculation.
Ex. sum of [2,6] - we don't have it stored.
However, we have [0,6] = [0,1] + [2,6]
Thus, [2,6] = [0,6] - [0,1] - which values we have stored = a[6] - a[1]

A[i,j] = A[j] - A[i - 1]

It takes O(n) time to calc prefix sum array of n size array.
It takes O(1) time to calc range sum query of n size array.