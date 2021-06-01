---
title: 检查数组对是否可以被 k 整除
tags: ['算法', '贪心算法']
categories: 算法
date: 2020-08-31
---

给你一个整数数组 arr 和一个整数 k ，其中数组长度是偶数，值为 n 。

现在需要把数组恰好分成 n / 2 对，以使每对数字的和都能够被 k 整除。

如果存在这样的分法，请返回 True ；否则，返回 False 。

<!--more-->

示例 1：
输入：arr = [1,2,3,4,5,10,6,7,8,9], k = 5
输出：true
解释：划分后的数字对为 (1,9),(2,8),(3,7),(4,6) 以及 (5,10) 。

示例 2：
输入：arr = [1,2,3,4,5,6], k = 7
输出：true
解释：划分后的数字对为 (1,6),(2,5) 以及 (3,4) 。

示例 3：
输入：arr = [1,2,3,4,5,6], k = 10
输出：false
解释：无法在将数组中的数字分为三对的同时满足每对数字和能够被 10 整除的条件。

示例 4：
输入：arr = [-10,10], k = 2
输出：true

示例 5：
输入：arr = [-1,1,-2,2,-3,3,-4,4], k = 3
输出：true

提示：
arr.length == n
1 <= n <= 10^5
n 为偶数
-10^9 <= arr[i] <= 10^9
1 <= k <= 10^5

妈耶，这道题，感觉难点不是在贪心算法，而是在参数非常大时如何对传入的参数进行优化。

首先上一下自己的解题思路。画了 UML 图之后思路很清晰，一遍过。

但是问题就是太吃内存了。

![解法拓扑图](https://s1.ax1x.com/2020/08/31/dOnRrn.png)

题解的思路很好，使用了 map 的数据结构，然后取余数作为 map 的 key。

自己的解法：

```javascript
/**
 * @param {number[]} arr
 * @param {number} k
 * @return {boolean}
 */
var canArrange = function (arr, k) {
  let sortArr = arr.sort((a, b) => {
    return a - b
  })
  let left = 0
  let right = sortArr.length - 1
  while (true) {
    if (sortArr.length === 0) return true
    if (left === right) return false
    let currSum = sortArr[left] + sortArr[right]
    if (currSum % k === 0) {
      sortArr.splice(0, 1)
      sortArr.splice(right - 1, 1)
      right = sortArr.length - 1
    } else {
      right -= 1
    }
  }
}
```

题解：

```javascript
/**
 * @param {number[]} arr
 * @param {number} k
 * @return {boolean}
 */
var canArrange = function (arr, k) {
  let map = {},
    len = arr.length

  // 统计所有余数的出现次数到 map 中
  for (let i = 0; i < len; i++) {
    let remain = arr[i] % k
    if (remain < 0) remain += k
    map[remain] === undefined ? (map[remain] = 1) : (map[remain] += 1)
  }

  // 遍历所有余数，看看是否对于每个余数 x，在 map 中都有相同出现次数的 k-x
  let keys = Object.keys(map),
    n = keys.length
  let i = 0
  while (i < n) {
    let key = keys[i]
    if (map[key] === 0) {
      // 处理过了，跳过
      i++
      continue
    }
    if (key == '0' && map[key] % 2 === 0) {
      // 本身余数为 0，自己的出现次数是偶数即可
      map[key] = 0
    } else if (map[k - Number(key)] === map[key]) {
      // map[x] === map[k-x]
      map[key] = 0
      map[k - Number(key)] = 0
    } else {
      // 一旦不满足返回 false
      return false
    }
    i++
  }
  return true
}
```

咳咳，来更新下，这道题始终没跑过，今天又改了一下，发现自己的思路真的是 low 爆了，而且又发现老婆果然厉害，她的题解目前排在第一位，我也算是终于理解了她的思路。先补一下自己的做法：

```javascript
/**
 * @param {number[]} arr
 * @param {number} k
 * @return {boolean}
 */
var canArrange = function (arr, k) {
  let map = new Map()
  for (let i = 0; i < arr.length; i++) {
    let key = arr[i] % k
    let keyTrans, complementary

    // 下面这个语句是根据当前值将当前值转化为小于k的非负数
    if (key < 0) {
      keyTrans = k + key
      complementary = k - keyTrans
      console.log(arr[i], key, k, keyTrans, complementary)
    } else if (key === 0) {
      keyTrans = 0
      complementary = 0
    } else {
      keyTrans = key
      complementary = k - keyTrans
    }

    // 然后判断当前值的补数是否存在于map中，如果存在，删除map中的值，否则放进map里
    if (map.has(complementary)) {
      let num = map.get(complementary)
      if (num === 1) {
        map.delete(complementary)
      } else {
        map.set(complementary, num - 1)
      }
    } else {
      if (map.has(keyTrans)) {
        let num = map.get(keyTrans) + 1
        map.set(keyTrans, num)
      } else {
        map.set(keyTrans, 1)
      }
    }
    console.log(map)
  }
  return map.size === 0
}
let arr = [-4, -7, 5, 2, 9, 1, 10, 4, -8, -3]
k = 3
console.log(canArrange(arr, k))
```
