---
title: 平衡字符串
tags: ['算法', '贪心算法']
categories: 算法
---

在一个「平衡字符串」中，'L' 和 'R' 字符的数量是相同的。

给出一个平衡字符串  s，请你将它分割成尽可能多的平衡字符串。

返回可以通过分割得到的平衡字符串的最大数量。

<!--more-->

示例 1：
输入：s = "RLRRLLRLRL"
输出：4
解释：s 可以分割为 "RL", "RRLL", "RL", "RL", 每个子字符串中都包含相同数量的 'L' 和 'R'。

示例 2：
输入：s = "RLLLLRRRLR"
输出：3
解释：s 可以分割为 "RL", "LLLRRR", "LR", 每个子字符串中都包含相同数量的 'L' 和 'R'。

示例 3：
输入：s = "LLLLRRRR"
输出：1
解释：s 只能保持原样 "LLLLRRRR".

提示：
1 <= s.length <= 1000
s[i] = 'L' 或 'R'
分割得到的每个字符串都必须是平衡字符串。

这道题题目描述的有点不清晰，按照给的示例，应该所有的平衡字符串都是左右对称的，然鹅按照题解，只要连续的几个"L"的数量和"R"相同即可。

先是按照我自己的思路搞了一下，我的思路就是所有平衡字符串一定有一堆相邻的"LR"或者"RL"。以这个为判断平衡字符串的个数。

```javascript
/**
 * @param {string} s
 * @return {number}
 */
var balancedStringSplit = function (s) {
  let sArr = s.split('')
  let tempArr = []
  let balancedStrNum = 0
  for (let i = 0; i < sArr.length; i++) {
    if (tempArr.length > 0) {
      let tempSum = sArr[i] + tempArr[tempArr.length - 1]
      if (tempSum === 'LR' || tempSum === 'RL') {
        balancedStrNum += 1
        tempArr = []
        continue
      }
    }
    tempArr.push(sArr[i])
  }
  return balancedStrNum
}
```

然后根据题解的思路又写了一下，很简单。

```javascript
/**
 * @param {string} s
 * @return {number}
 */
var balancedStringSplit = function (s) {
  let sumArr = 0
  let balancedStrNum = 0
  for (let i = 0; i < s.length; i++) {
    sumArr += s[i] === 'R' ? -1 : 1
    if (sumArr === 0) balancedStrNum += 1
  }
  return balancedStrNum
}
let s = 'LLLLRRRR'
console.log(balancedStringSplit(s))
```

值得说的就是这个二元运算符的写法，这样就少写了一个 if else。
