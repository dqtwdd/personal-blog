---
title: 使括号有效的最少添加
tags: ['算法', '动态规划']
categories: 算法
---

给定一个由  '('  和  ')'  括号组成的字符串 S，我们需要添加最少的括号（ '('  或是  ')'，可以在任何位置），以使得到的括号字符串有效。

从形式上讲，只有满足下面几点之一，括号字符串才是有效的：

它是一个空字符串，或者它可以被写成  AB （A  与  B  连接）, 其中  A 和  B  都是有效字符串，或者它可以被写作  (A)，其中  A  是有效字符串。

给定一个括号字符串，返回为使结果字符串有效而必须添加的最少括号数。

<!--more-->

示例 1：
输入："())"
输出：1

示例 2：
输入："((("
输出：3

示例 3：
输入："()"
输出：0

示例 4：
输入："()))(("
输出：4

提示：
S.length <= 1000
S 只包含  '(' 和  ')'  字符。

中等难度的题，但是在做过有效括号的问题后，这道题就显得很简单咯！
给出两种方法

```javascript
/**
 * @param {string} S
 * @return {number}
 */
// 方法1
var minAddToMakeValid = function (S) {
  while (S.includes('()')) {
    S = S.replace('()', '')
  }
  return S.length
}
// 方法2
var minAddToMakeValid = function (S) {
  let res = []
  for (let i = 0; i < S.length; i++) {
    if (res[res.length - 1] + S[i] === '()') {
      res.pop()
    } else {
      res.push(S[i])
    }
  }
  return res.length
}
let s = '((())'
console.log(minAddToMakeValid(s))
```
