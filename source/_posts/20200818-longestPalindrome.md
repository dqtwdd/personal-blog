---
title: 最长回文子序列，亲密字符串
tags: ['算法']
categories: 算法
date: 2020-08-18
---
### 最长回文子串

给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

<!--more-->

示例 1：

输入: "babad"

输出: "bab"

注意: "aba" 也是一个有效答案。

示例 2：

输入: "cbbd"

输出: "bb"

这道题是中等难度，本来是不会做的，参考了答案之后学会了使用动态规划方法解决。

主要思路就是双层for循环，判断两个值是否相等，然后创建二维数组，两个坐标分别对应两层for循环的key，如果相等，给对应二维数组的位置赋值true，证明两个坐标之间为回文序列。需要分三种情况考虑：

1.i 和 j相同，就是同一个数字，直接给dp赋值true。

2.i 和 j相差1，就是两个数字相邻，也是直接给dp赋值true。

3.第三种情况对应大部分情况，就是判断两边的数字相同，且两边夹着的内部的最外面的两个坐标对应的dp是true，既里面是回文序列，外面还相同，这样就是一个回文序列。

```javascript
/**
 * @param {string} s
 * @return {string}
 */
var longestPalindrome = function(s) {
  if (!s || s.length === 0) return '' 
  let sArr = s.split('')
  let dp = []
  let res = sArr[0]
  for (let i = sArr.length; i >= 0; i--) {
    dp[i] = []
    for (let j = i; j < sArr.length; j++) {
      if (i === j) {
        dp[i][j] = true
      } else if (j - i === 1 && sArr[i] === sArr[j]) {
        dp[i][j] = true
      } else if (sArr[i] === sArr[j] && dp[i + 1][j - 1]) {
        dp[i][j] = true
      }
      if (dp[i][j] && j - i + 1 > res.length) res = sArr.slice(i,j+1)
    } 
  }
  if (res.length <= 1) return res
  return res.join('')
};
let s = 'ac'
console.log(longestPalindrome(s))

```

### 亲密字符串

给定两个由小写字母构成的字符串 A 和 B ，只要我们可以通过交换 A 中的两个字母得到与 B 相等的结果，就返回 true ；否则返回 false 。

示例 1：

输入： A = "ab", B = "ba"
输出： true
示例 2：

输入： A = "ab", B = "ab"
输出： false
示例 3:

输入： A = "aa", B = "aa"
输出： true
示例 4：

输入： A = "aaaaaaabc", B = "aaaaaaacb"
输出： true
示例 5：

输入： A = "", B = "aa"
输出： false

这道题有点意思，但是也比较简单，只需要考虑全面情况就可以了。

1.如果字符串长度为0或者小于2，肯定不是。

2.如果字符串长度不一致，肯定不是。

3.如果字符串长度一致，有且只有两个不一致的地方，而且两个不一样的位置的值互换相同，返回true。

4.如果两个字符串完全相同，且单个字符串中没有相同元素，肯定不是；如果有一个或一个以上相同元素，返回true。

```javascript
/**
 * @param {string} A
 * @param {string} B
 * @return {boolean}
 */
var buddyStrings = function(A, B) {
    let aArr = []
    let bArr = []
    if (A.length !== B.length) return false // 2
    let aSet = [...new Set(A)]
    if (aSet.length !== A.length && A === B) {
        return true // 4
    }
    for (let i = 0; i < A.length; i++) {
        if (A[i] !== B[i]) {
            aArr.push(A[i])
            bArr.push(B[i])
        }
    }
    if (aArr.length > 2 || aArr.length === 0 || A === B) return false // 1
    if (aArr[1] === bArr[0] && aArr[0] === bArr[1]) {
        return true // 3
    } else {
        return false //3
    }
};
```

