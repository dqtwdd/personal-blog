---
title: 一和零
tags: ['算法', '动态规划']
categories: 算法
date: 2020-09-15
---

在计算机界中，我们总是追求用有限的资源获取最大的收益。

现在，假设你分别支配着 m 个  0  和 n 个  1。另外，还有一个仅包含  0  和  1  字符串的数组。

你的任务是使用给定的  m 个  0  和 n 个  1 ，找到能拼出存在于数组中的字符串的最大数量。每个  0  和  1  至多被使用一次。

<!--more-->

注意:

给定  0  和  1  的数量都不会超过  100。
给定字符串数组的长度不会超过  600。

示例 1:
输入: Array = {"10", "0001", "111001", "1", "0"}, m = 5, n = 3
输出: 4
解释: 总共 4 个字符串可以通过 5 个 0 和 3 个 1 拼出，即 "10","0001","1","0" 。

示例 2:
输入: Array = {"10", "0", "1"}, m = 1, n = 1
输出: 2
解释: 你可以拼出 "10"，但之后就没有剩余数字了。更好的选择是拼出 "0" 和 "1" 。

咳咳。我宣布，我已经掌握了动态规划的精髓！！！！就一句话：**一看就会，一做就不对！!**

真叫人脑壳痛，动态规划做了也有十几道了，还是不太能摸清规律。

不发牢骚啦，回归本题，这道题属于是比较有代表性的，属于背包问题。

看了很多篇文章，以下这篇文章讲的最好

[背包问题图解](https://www.jianshu.com/p/a66d5ce49df5)

然后回归本题，个人认为比较值得记录的就是背包问题的解题办法，就是绘表法。

举个例子，比如本题中输入参数 `Array=[10,01,101,0], m=3, n=2`

算了还是看题解的例子吧，感觉题解这个画表画的就很好。

[一和零力扣官方图解](https://leetcode-cn.com/problems/ones-and-zeroes/solution/yi-he-ling-by-leetcode/)

需要注意的就是这道题是从右下角往左上角画的。这个留个疑，不太理解，如果以后练习过程中理解了再回来记录下。

上一下代码

```javascript
/**
 * @param {string[]} strs
 * @param {number} m
 * @param {number} n
 * @return {number}
 */
/**
 * 对于列表中遍历的每一个项目，都有两种情况：
 *  1.取当前值    取前值时，最大值为  当前货物的价值 + （总体积-当前货物体积）体积下货物的最大价值
 *  2.放弃当前值  放弃当前值时，不放入
 * 转移态方程
 * dp[i][j] = Math.max(dp[i][j],dp[i - strsItemZero][j - strsItemOne])
 */
var findMaxForm = function (strs, m, n) {
  let max = 0
  let getOneNum = function (item) {
    return item.replace(/0/g, '').length
  }
  let tempArr = []
  let dp = []
  for (let a = 0; a <= m; a++) {
    dp[a] = []
    for (let b = 0; b <= n; b++) {
      dp[a][b] = 0
    }
  }
  console.log(dp)
  for (let i = 0; i < strs.length; i++) {
    let strsItem = strs[i]
    strsItemOne = getOneNum(strsItem)
    strsItemZero = strsItem.length - strsItemOne
    // 画表 0 => j, 1 => k
    for (let j = m; j >= strsItemZero; j--) {
      for (let k = n; k >= strsItemOne; k--) {
        if (strsItemZero <= j && strsItemOne <= k) {
          dp[j][k] = Math.max(
            dp[j][k],
            1 + dp[j - strsItemZero][k - strsItemOne]
          )
        } else {
          dp[j][k] = 0
        }
        console.log('dp:', dp)
      }
    }
  }
  return dp[m][n]
}
let Array = ['10', '01', '101', '0'],
  m = 3,
  n = 2
console.log(findMaxForm(Array, m, n))
```
