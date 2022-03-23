---
title: 双指针算法合集
tags: ['算法','双指针']
categories: 算法
date: 2021-12-21
---

双指针算法合集。

<!--more-->

### 盛最多水的容器

给你 `n` 个非负整数 `a1，a2，...，a``n`，每个数代表坐标中的一个点 `(i, ai)` 。在坐标内画 `n` 条垂直线，垂直线 `i` 的两个端点分别为 `(i, ai)` 和 `(i, 0)` 。找出其中的两条线，使得它们与 `x` 轴共同构成的容器可以容纳最多的水。

**说明：**你不能倾斜容器。 

**示例 1：**

![img](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/07/25/question_11.jpg)

```
输入：[1,8,6,2,5,4,8,3,7]
输出：49 
解释：图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。
```

**示例 2：**

```
输入：height = [1,1]
输出：1
```

**示例 3：**

```
输入：height = [4,3,2,1,4]
输出：16
```

**示例 4：**

```
输入：height = [1,2,1]
输出：2 
```

**提示：**

- `n == height.length`
- `2 <= n <= 105`
- `0 <= height[i] <= 104`

从最两边往中间走，哪边比较小哪边往中间走，直到左右两边指针重合。

```javascript
/**
 * @param {number[]} height
 * @return {number}
 */
var maxArea = function(height) {
  let left = 0
  let right = height.length - 1
  let max = 0
  while (left !== right) {
    let curr = (right - left) * Math.min(height[left],height[right])
    if (curr>max) {
      max = curr
    }
    if (height[left]<height[right]) {
      left += 1
    } else {
      right -= 1
    }
  }
  return max
};
```

### 三数之和

给你一个包含 `n` 个整数的数组 `nums`，判断 `nums` 中是否存在三个元素 *a，b，c ，*使得 *a + b + c =* 0 ？请你找出所有和为 `0` 且不重复的三元组。

**注意：**答案中不可以包含重复的三元组。

**示例 1：**

```
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
```

**示例 2：**

```
输入：nums = []
输出：[]
```

**示例 3：**

```
输入：nums = [0]
输出：[]
```

**提示：**

- `0 <= nums.length <= 3000`
- `-105 <= nums[i] <= 105`

```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var threeSum = function(nums) {
  let len = nums.length
  if (len<3) return [] // 长度不足3个直接返回
  nums.sort((a,b)=>{return a-b}) // 排序一下
  let res = []
  for (let i = 0; i < len; i++) {
    let L = i+1 // 左指针
    let R = len-1 // 右指针
    if (nums[i] > 0) break // 排序后如果当前值大于0后面的一定比0大，直接跳出循环
    if (i > 0 && nums[i] === nums[i-1]) continue // 当前值如果之前有过的话直接跳过，去重
    while (L < R) { //当左指针右指针不重合时循环
      let SUM = nums[i] + nums[L] + nums[R] // 记和
    	if (SUM === 0) { // 如果和为0时
        res.push([nums[i],nums[L],nums[R]]) // 将答案push进答案数组
        while (L<R && nums[L] === nums[L+1]) L++ // 当左指针的下一个和当前左指针的值相等，左指针右移，去重
        while (L<R && nums[R] === nums[R-1]) R-- // 当右指针的前一个和当前右指针的值相等，右指针左移，去重
        L++ // 左指针移动
        R-- // 右指针移动
      } 
      else if (SUM < 0) L++ // 当和小于0时，说明总体小了，左指针右移
      else if (SUM > 0) R-- // 当和大于0时，说明总体大了，右指针左移
    }
  }
  return res // 返回结果
}
```

