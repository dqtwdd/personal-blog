---
title: 三步问题
tags: ['算法']
categories: 算法
date: 2020-08-20
---

三步问题。有个小孩正在上楼梯，楼梯有n阶台阶，小孩一次可以上1阶、2阶或3阶。实现一种方法，计算小孩有多少种上楼梯的方式。结果可能很大，你需要对结果模1000000007。

<!--more-->

示例1:

 输入：n = 3 

 输出：4

 说明: 有四种走法

示例2:

 输入：n = 5

 输出：13

提示:

n范围在[1, 1000000]之间

哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈

不好意思，先让我笑一会，因为这道题，我的第一个解法实在是。

让我自己都不好意思说自己是一个程序员。

第一个方法，我竟然没有马上想到使用动态规划，而是使用了一个蠢方法，暴力递归。

有多暴力呢。你瞅瞅：

```javascript
/**
 * @param {number} n
 * @return {number}
 */
var waysToStep = function(n) {
  let res = 0
  let current = 0
  let walk = function (temp) {
    for (let i = 1; i <= 3; i++) { 
      current = temp + i
      if (current === n) {
        res++
      } else if (current < n) {
        walk(temp + i)
      }
    }
  }
  walk(0)
  return res
};
```
核心思想就是创建一个walk函数，传入参数是当前走了几步，然后函数中遍历123，模拟一次走一步两步三步，然后如果当前迈步的大小正好等于整个台阶阶数，就给结果数量+1，如果小于台阶数量，就继续一步两步三步去递归。

导致的结果就是当n = 20时我的电脑就崩了，res的每一个结果都是一个一个加上去的。

如果有哪个倒霉的小朋友按照我的代码去走这个楼梯一定能活活累死。

哈哈哈哈哈哈哈哈哈哈哈

---

好了，正经的动态规划来了。其实很简单，跟计算斐波那契数列的方法类似，只要注意一下题目中提出了n的范围，所以在循环的时候注意一下就好了。

```javascript
/**
 * @param {number} n
 * @return {number}
 */
var waysToStep = function(n) {
  let dp = []
  dp[1] = 1
  dp[2] = 2
  dp[3] = 4
  for (let i = 4; i <= (n < 1000000? n:1000000); i++) {
    dp[i] = (dp[i - 1] + dp[i - 2] + dp[i - 3]) % 1000000007
  }
  return dp[n]
}
```