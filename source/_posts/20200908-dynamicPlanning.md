---
title: 动态规划题目合集[按摩师，打家劫舍，判断子序列，连续的子数组的和]
tags: ['算法', '动态规划']
categories: 算法
---

## 思路解析

首先让我贴上一篇我看到对动态规划最好的解释：

How should i explain Dynamic Programming to a 4-year-old?

_writes down "1+1+1+1+1+1+1+1 =" on a sheet of paper_
"What's that equal to?"
_counting_ "Eight!"
_writes down another "1+" on the left_
"What about that?"
_quickly_ "Nine!"
"How'd you know it was nine so fast?"
"You just added one more"
"So you didn't need to recount because you remembered there were eight! Dynamic Programming is just a fancy way to say 'remembering stuff to save time later'"

<!--more-->

按摩师和打家劫舍问题几乎就是一模一样的，通过这两道题找到动态规划一个很重要的解题思路，就是读题时要关注出现的数字，比如三步问题中出现了“三”，最后动态规划就是最后一个值是`dp[i-1]+dp[i-2]+dp[i-3]`,如果最后出现了“相邻”，一般最后结果都是和签名两个相关，比如打家劫舍的和按摩师的`dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i])`，有了这个王老师的生活小技巧可以很大程度上帮助你迅速建立数学模型。

## 按摩师

一个有名的按摩师会收到源源不断的预约请求，每个预约都可以选择接或不接。在每次预约服务之间要有休息时间，因此她不能接受相邻的预约。给定一个预约请求序列，替按摩师找到最优的预约集合（总预约时间最长），返回总的分钟数。

注意：本题相对原题稍作改动

示例 1：
输入： [1,2,3,1]
输出： 4
解释： 选择 1 号预约和 3 号预约，总时长 = 1 + 3 = 4。

示例 2：
输入： [2,7,9,3,1]
输出： 12
解释： 选择 1 号预约、 3 号预约和 5 号预约，总时长 = 2 + 9 + 1 = 12。

示例 3：
输入： [2,1,4,5,3,1,1,3]
输出： 12
解释： 选择 1 号预约、 3 号预约、 5 号预约和 8 号预约，总时长 = 2 + 4 + 3 + 3 = 12。

```javascript
// 按摩师
/**
 * @param {number[]} nums
 * @return {number}
 */
var massage = function (nums) {
  if (nums.length === 0) return 0
  if (nums.length === 1) return nums[0]
  if (nums.length === 2) return Math.max(nums[0], nums[1])
  let dp = [nums[0], Math.max(nums[0], nums[1])]
  for (let i = 2; i < nums.length; i++) {
    dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i])
  }
  return dp[dp.length - 1]
}
```

## 打家劫舍

你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。

示例 1：
输入：[1,2,3,1]
输出：4
解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
  偷窃到的最高金额 = 1 + 3 = 4 。

示例 2：
输入：[2,7,9,3,1]
输出：12
解释：偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
  偷窃到的最高金额 = 2 + 9 + 1 = 12 。

提示：
0 <= nums.length <= 100
0 <= nums[i] <= 400

```javascript
// 打家劫舍
/**
 * @param {number[]} nums
 * @return {number}
 */
var rob = function (nums) {
  if (nums.length === 0) return 0
  if (nums.length === 1) return nums[0]
  if (nums.length === 2) return Math.max(nums[0], nums[1])
  let maxArr = [nums[0], Math.max(nums[0], nums[1])]
  for (let i = 2; i < nums.length; i++) {
    maxArr[i] = Math.max(maxArr[i - 1], maxArr[i - 2] + nums[i])
  }
  return maxArr[nums.length - 1]
}
```

## 判断子序列

给定字符串 s 和 t ，判断 s 是否为 t 的子序列。

你可以认为 s 和 t 中仅包含英文小写字母。字符串 t 可能会很长（长度 ~= 500,000），而 s 是个短字符串（长度 <=100）。

字符串的一个子序列是原始字符串删除一些（也可以不删除）字符而不改变剩余字符相对位置形成的新字符串。（例如，"ace"是"abcde"的一个子序列，而"aec"不是）。

示例  1:
s = "abc", t = "ahbgdc"
返回  true.

示例  2:
s = "axc", t = "ahbgdc"
返回  false.

后续挑战 :

如果有大量输入的 S，称作 S1, S2, ... , Sk 其中 k >= 10 亿，你需要依次检查它们是否为 T 的子序列。在这种情况下，你会怎样改变代码？

这道题可以使用双指针的思想，因为子字符串在父字符串中出现的顺序一定是顺序的，所以子字符串的指针到最后的就可以判断为 true 了。

```javascript
/**
 * @param {string} s
 * @param {string} t
 * @return {boolean}
 */
var isSubsequence = function (s, t) {
  if (s.length === 0) return true
  if (t.length === 0 || t.length < s.length) return false
  let i = 0
  let currLetter = s[i]
  for (let j = 0; j < t.length; j++) {
    if (t[j] === currLetter) {
      i += 1
      if (i >= s.length) {
        return true
      }
      currLetter = s[i]
    }
  }
  return false
}
```

## 连续的子数组和

给定一个包含 非负数 的数组和一个目标 整数  k，编写一个函数来判断该数组是否含有连续的子数组，其大小至少为 2，且总和为 k 的倍数，即总和为 n\*k，其中 n 也是一个整数。

示例 1：
输入：[23,2,4,6,7], k = 6
输出：True
解释：[2,4] 是一个大小为 2 的子数组，并且和为 6。

示例 2：
输入：[23,2,6,4,7], k = 6
输出：True
解释：[23,2,6,4,7]是大小为 5 的子数组，并且和为 42。

这道题我看到之后想到的是使用二维数组，然后每次后一个的值都可以根据前一个的值计算，需要注意的就是考虑 k 为 0。

```javascript
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {boolean}
 */
var checkSubarraySum = function (nums, k) {
  if (nums.length < 2) return false
  let dp = []
  for (let i = 0; i < nums.length - 1; i++) {
    dp[i] = []
    dp[i][i] = [nums[i]]
    for (let j = i + 1; j < nums.length; j++) {
      dp[i][j] = 1 * dp[i][j - 1] + nums[j]
      if (dp[i][j] % k === 0 || (k === 0 && dp[i][j] === 0)) {
        return true
      }
    }
  }
  return false
}
```
