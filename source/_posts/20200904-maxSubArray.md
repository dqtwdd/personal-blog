---
title: 最大子序和
tags: ['算法', '动态规划']
categories: 算法
date: 2020-09-04
---

给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

<!--more-->

示例:
输入: [-2,1,-3,4,-1,2,1,-5,4]
输出: 6
解释:  连续子数组  [4,-1,2,1] 的和最大，为  6。

进阶:
如果你已经实现复杂度为 O(n) 的解法，尝试使用更为精妙的分治法求解。

这道题很简单，感觉动态规划的题如果有了思路的话会很简单，路子都很接近，直接上代码：

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxSubArray = function (nums) {
  let dp = nums[0],
    max = nums[0]
  let right = 1
  while (right < nums.length) {
    dp = Math.max(dp + nums[right], nums[right])
    max = Math.max(dp, max)
    right++
  }
  return max
}
```
