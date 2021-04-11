---
title: 用JS实现各大排序算法(上)
date: 2021-04-11 12:38:34
tags:
---

### 常见的排序算法
冒泡排序、插入排序、选择排序、快速排序、归并排序。。。
### 一些排序算法的概念
- **排序算法的执行效率分析**
   1. 最好情况、最坏情况、平均情况时间复杂度
   2. 时间复杂度的系数、常数 、低阶
   3. 比较次数和交换（或移动）次数
   
- **原地排序**
   空间复杂度为O(1)的排序算法（即除了数据存储本身的空间不需要额外的辅助空间）。冒泡、插入、选择都是`原地排序`的算法 

- **排序算法的稳定性**
  待排序的序列中值相等的元素，在经过排序算法排序之后，原有的`先后顺序`保持不变

### 冒泡排序（Bubble Sort）
- 时间复杂度：最好O(n), 最差O(n^2)
- 是否原地排序：✅
- 是否稳定排序：✅
- 思路：相邻的元素一一比较

#### 代码实现
``` js
/** 冒泡排序 */
const bubbleSort = arr => {
  let swapped = false
  for (let i = 1; i < arr.length - 1; i++) {
    for (let j = 0; j < arr.length - i; j ++) {
      if (arr[ j + 1] < arr[j]) {
        [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
        swapped = true
      }
    }
    if(!swapped) {
      return arr
    }
    return arr
  }
}
```

### 插入排序（Insertion Sort）
- 时间复杂度：最好O(n), 最差O(n^2)
- 是否原地排序：✅
- 是否稳定排序：✅
- 思路：将数据分为**已排序区间**和**未排序区间**，取未排序区间的元素在已排序区间中找到合适的位置插入，并且保持已排序区间的数据一直有序

#### 代码实现
``` js
/** 插入排序 */

const insertionSort = arr => {
  const length = arr.length;
  let tem = 0;
  for (let i = 1; i < length; i ++) {
    tem = arr[i]
    let j
    for (j = i -1; j >= 0; j --) {
      if (tem < arr[j]) {
        arr[j + 1] = arr[j];
        continue
      }
      break
    }
    arr[j + 1] = temp
  }
  return arr
}
```

### 选择排序（Insertion Sort）
- 时间复杂度：最好最差都是O(n^2)
- 是否原地排序：✅
- 是否稳定排序：❌
- 思路：将数据分为**已排序区间**和**未排序区间**，取未排序区间的元素中最小的元素，把它放在已排序区间的末尾

#### 代码实现
``` js
/** 插入排序 */

const insertionSort = arr => {
  const length = arr.length;
  let tem = 0;
  for (let i = 1; i < length; i ++) {
    tem = arr[i]
    let j
    for (j = i -1; j >= 0; j --) {
      if (tem < arr[j]) {
        arr[j + 1] = arr[j];
        continue
      }
      break
    }
    arr[j + 1] = temp
  }
  return arr
}

```
### 总结

![348604caaf0a1b1d7fee0512822f0e50](https://cdn.jsdelivr.net/gh/oliver34/image-host@main/blog/348604caaf0a1b1d7fee0512822f0e50.5obh29r1yvs0.jpeg)