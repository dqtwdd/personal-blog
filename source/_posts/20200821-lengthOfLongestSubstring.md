---
title: 无重复字符的最长子串
tags: ['算法']
categories: 算法
date: 2020-08-21
---

给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

<!--more-->

示例 1:

输入: "abcabcbb"

输出: 3 

解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

示例 2:

输入: "bbbbb"

输出: 1

解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。

示例 3:

输入: "pwwkew"

输出: 3

解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。

     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。

emmmmmm...

这道题嘛。本来是不太会的，可是做了另一道比较类似的题，最长回文子字符串，然后这道题也就会了。

但是坏处是，虽然之前的思路可以给我一个思考的方向，让我做出了这道题，但是同时也束缚了我的思路，导致我的解法并不是最优解。

还是媳妇厉害。

先贴上我自己的写法：

```javascript
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function(s) {
  let longestRes = 0
  for (let i = 0; i < s.length; i++) {
    for (let j = i; j < s.length; j++) {
      let subStr = s.slice(i,j)
      let tempLetter = s[j]
      if (!subStr.includes(tempLetter)) {
        if (j - i > longestRes) {
          longestRes = j - i
        }
      } else {
        break
      }
    }
  }
  if (s.length === 0) return 0
  return longestRes + 1
};
```

思想就是两层遍历，然后判断当前最长无重复字符串中是否有指针指向的这个字母，如果没有并且当前长度大于当前最大长度就存一下最长长度，否则就终止，外层指针往前挪一个。

这个办法显然是不够优秀的，更好的做法是在重复时直接将指针移向当前最长字符串中与当前字符重复的位置，然后继续向前。

这么优秀的办法，老婆一次做出来。下面贴一下她的写法。

```javascript
var lengthOfLongestSubstring = function(s) {
  let childStr = '';
  let max = 0;
  for (let i = 0; i < s.length; ++i) {
    const index = childStr.indexOf(s[i]);
    if (index === -1) {
      childStr += s[i];
    } else {
      max = Math.max(childStr.length,max);
      childStr = childStr.substring(index + 1) + s[i];
    }
    return max = Math. max (childStr. length, max);
};
```
