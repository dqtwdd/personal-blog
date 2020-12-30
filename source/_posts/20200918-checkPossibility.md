---
title: 非递减数列
tags: ['算法', '动态规划']
categories: 算法
---

给你一个长度为 n 的整数数组，请你判断在 最多 改变 1 个元素的情况下，该数组能否变成一个非递减数列。

我们是这样定义一个非递减数列的： 对于数组中所有的 i (0 <= i <= n-2)，总满足 nums[i] <= nums[i + 1]。

<!--more-->

示例 1:
输入: nums = [4,2,3]
输出: true
解释: 你可以通过把第一个4变成1来使得它成为一个非递减数列。

示例 2:
输入: nums = [4,2,1]
输出: false
解释: 你不能在只改变一个元素的情况下将其变为非递减数列。

说明：
1 <= n <= 10 ^ 4
- 10 ^ 5 <= nums[i] <= 10 ^ 5

这道简单题困扰了我很久，但是最后还是被我做出来了啊哈哈哈哈。

头几天状态不太好，所以做题思路不明确，后来思路明确了就简单了。

其实这道题最重要的思想就是找到拐点，然后判断，如果拐点大于1就返回false。

如果拐点等于1，就判断一下是否可以改变一个数字形成非递减数组。

题解的思路很清晰，跟我的区别是我将拐点定义为变小的那个数。题解的拐点定义为变小的数前一个的数字。

写法还有优化空间，但是没有优化。先这样吧。

```javascript
/**
 * @param {number[]} nums
 * @return {boolean}
 */
var checkPossibility = function (nums) {
  let breakPointList = []
  if (nums.length === 1 ||nums.length === 0 ) {
    return true
  }
  for (let i = 1; i < nums.length; i ++) {
    if (nums[i] < nums[i-1]) {
      breakPointList.push(i)
    }
  }
  if (breakPointList.length > 1) {
    console.log('---')
    return false
  }  else if (breakPointList[0] === 1 || breakPointList[0] === nums.length-1){
    return true
  } else if (nums[breakPointList[0]+1]>= nums[breakPointList[0]-1] ||(nums[breakPointList[0]]>= nums[breakPointList[0]-2] &&nums[breakPointList[0]+1]>= nums[breakPointList[0]-2] )) {
    return true
  } else if (breakPointList.length === 0) {
    return true
  } else {
    return false
  }
}

let nums = [1,2,3]
console.log(checkPossibility(nums))
```
