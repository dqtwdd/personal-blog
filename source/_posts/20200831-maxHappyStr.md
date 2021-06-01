---
title: 最长快乐字符串
tags: ['算法', '贪心算法']
categories: 算法
date: 2020-08-31
---

如果字符串中不含有任何 'aaa'，'bbb' 或 'ccc' 这样的字符串作为子串，那么该字符串就是一个「快乐字符串」。

给你三个整数 a，b ，c，请你返回 任意一个 满足下列全部条件的字符串 s：

s 是一个尽可能长的快乐字符串。
s 中 最多 有 a 个字母 'a'、b  个字母 'b'、c 个字母 'c' 。
s 中只含有 'a'、'b' 、'c' 三种字母。
如果不存在这样的字符串 s ，请返回一个空字符串 ""。

<!--more-->

示例 1：

输入：a = 1, b = 1, c = 7
输出："ccaccbcc"
解释："ccbccacc" 也是一种正确答案。
示例 2：

输入：a = 2, b = 2, c = 1
输出："aabbc"
示例 3：

输入：a = 7, b = 1, c = 0
输出："aabaa"
解释：这是该测试用例的唯一正确答案。

提示：

0 <= a, b, c <= 100
a + b + c > 0

示例 1：

输入：A = [4,2,3], K = 1
输出：5
解释：选择索引 (1,) ，然后 A 变为 [4,-2,3]。
示例 2：

输入：A = [3,-1,0,2], K = 3
输出：6
解释：选择索引 (1, 2, 2) ，然后 A 变为 [3,1,0,2]。
示例 3：

输入：A = [2,-3,-1,5,-4], K = 2
输出：13
解释：选择索引 (1, 4) ，然后 A 变为 [2,3,-1,5,4]。

提示：

1 <= A.length <= 10000
1 <= K <= 10000
-100 <= A[i] <= 100

今天分享的是一道最长快乐字符串，做这道题，有一个非常重要的思路，就是当前要开始增加的字符串是次长的字符串时，要每次加一个。

给我的启示就是做一道题的时候一定要找到正确的思路然后再开始搞代码，然后才 ok，不然路子就走歪了，怎么写都是错的。

![解法拓扑图](https://s1.ax1x.com/2020/08/31/dL6lBn.png)

```javascript
/**
 * @param {number} a
 * @param {number} b
 * @param {number} c
 * @return {string}
 */
var longestDiverseString = function (a, b, c) {
  let arr = [
    { str: 'a', num: a },
    { str: 'b', num: b },
    { str: 'c', num: c }
  ]
  let sortArr = arr.sort((minItem, maxItem) => {
    return maxItem.num - minItem.num
  })
  let resStr = ''
  while (true) {
    let addStr = ''
    let addIndex = -1
    if (sortArr[0].str === resStr.slice(resStr.length - 1)) {
      addIndex = 1
      addStr = sortArr[1].str
    } else {
      addIndex = 0
      addStr = sortArr[0].str
    }
    if (sortArr[addIndex].num > 0) {
      if (addIndex === 0) {
        if (sortArr[addIndex].num > 1) {
          resStr += sortArr[addIndex].str + sortArr[addIndex].str
          sortArr[addIndex].num = sortArr[addIndex].num - 2
        } else {
          resStr += sortArr[addIndex].str
          sortArr[addIndex].num = sortArr[addIndex].num - 1
        }
      } else {
        resStr += sortArr[addIndex].str
        sortArr[addIndex].num = sortArr[addIndex].num - 1
      }
    } else {
      return resStr
    }
    sortArr = sortArr.sort((minItem, maxItem) => {
      return maxItem.num - minItem.num
    })
  }
}
console.log(longestDiverseString(0, 2, 5))
```
