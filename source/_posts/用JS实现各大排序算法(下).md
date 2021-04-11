---
title: 用JS实现各大排序算法(下)
date: 2021-04-11 15:14:26
tags:
---
快速排序、归并排序
<div align="center"> <img src="https://cdn.jsdelivr.net/gh/oliver34/image-host@main/blog/aa03ae570dace416127c9ccf9db8ac05.1zs7c0wdjjr4.jpeg" style="height: 220px"/></div>

<!--less-->

### 归并排序（Merge Sort）

- 时间复杂度：O(nlogn)
- 是否原地排序：❌ 空间复杂度 O(n)
- 是否稳定排序：✅
- 思路：分治思想，把数组从中间分成前后两部分，然后对前后两部分分别排序，再将排好序的两部分合并在一起，这样整个数组就都有序了。（递归）
- 图解：<div align="center"> <img src="https://cdn.jsdelivr.net/gh/oliver34/image-host@main/blog/mergeSort.5rh9jqblev0.png" style="height: 300px" /></div>

#### 代码实现

- 常规实现

```js
/** 归并排序 */
const merge = (left, right) => {
  const tempArr = [];
  while (left.length && right.length) {
    if (left[0] < right[0]) {
      tempArr.push(left.shift());
    } else {
      tempArr.push(right.shift());
    }
  }
  return tempArr.concat(left, right);
};

const mergeSort = (arr) => {
  if (arr.length < 2) {
    return arr;
  }

  const mid = Math.floor(arr.length / 2); // 取中间数
  const leftArr = mergeSort(arr.slice(0, mid));
  const rightArr = mergeSort(arr.slice(mid, arr.length));
  return merge(leftArr, rightArr);
};
```

- 更优雅的实现

```js
const mergeSort = (arr) => {
  if (arr.length < 2) return arr;
  const mid = Math.floor(arr.length / 2);
  const l = mergeSort(arr.slice(0, mid));
  const r = mergeSort(arr.slice(mid, arr.length));
  return Array.from({ length: l.length + r.length }, () => {
    if (!l.length) return r.shift();
    else if (!r.length) return l.shift();
    else return l[0] > r[0] ? r.shift() : l.shift();
  });
};
```

### 快速排序（Quick Sort）

- 时间复杂度：O(nlogn)
- 是否原地排序：✅ (根据实现方式区分)
- 是否稳定排序：❌
- 思路：
  1. 先从数组中找一个基准数
  2. 让其他比它大的元素移动到数列一边，比他小的元素移动到数列另一边，从而把数组拆解成两个部分。
  3. 再对左右区间重复第二步，直到各区间只有一个数。


#### 代码实现

- 非原地快排

```js
/** 非原地快排 */
const quickSort = (arr) => {
  if (arr.length < 2) {
    return arr;
  }
  const pivotIndex = Math.floor(arr.length < 2);
  const pivot = arr.splice(pivotIndex, 1)[0];
  const left = [];
  const right = [];
  for (let i = 0; i < arr.length; i++) {
    if (arr[i] < pivot) {
      left.push(arr[i]);
    } else {
      right.push(arr[i]);
    }
  }
  return [...quickSort(left), pivot, ...quickSort(right)];
};
```

- 非原地快排

```js
/** 原地快排 */
const quickSort1 = (arr) => {
  // 交换
  const swap = (swapArr, a, b) => {
    [swapArr[a], swapArr[b]] = [swapArr[b], swapArr[a]];
  }

  // 分区
  const partition = (partArr, left, right) => {
    const pivot = partArr[right];
    let storeIndex = left;
    for(let i = left; i < right; i++) {
      if (partArr[i] < pivot) {
        swap(partArr, storeIndex, i);
        storeIndex++
      }
    }
    swap(partArr, right, storeIndex);
    return storeIndex;
  }

  // 排序
  const sort = (sortArr, left, right) => {
    if(left > right) {
      return
    }
    const povit = partition(sortArr, left, right);
    sort(sortArr, left, povit - 1);
    sort(sortArr, povit + 1, right);
  }

  sort(arr, 0, arr.length - 1);
  return arr;
};
```

- 更优雅的实现

```js
const quickSort = arr => {
  const a = [...arr];
  if (a.length < 2) return a;
  const pivotIndex = Math.floor(arr.length / 2);
  const pivot = a[pivotIndex];
  const [lo, hi] = a.reduce(
    (acc, val, i) => {
      if (val < pivot || (val === pivot && i != pivotIndex)) {
        acc[0].push(val);
      } else if (val > pivot) {
        acc[1].push(val);
      }
      return acc;
    },
    [[], []]
  );
  return [...quickSort(lo), pivot, ...quickSort(hi)];
};
```
