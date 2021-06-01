---
title: 卡牌分组问题，求最大公约数
tags: ['算法']
categories: 算法
date: 2020-08-10
---
给定一副牌，每张牌上都写着一个整数。

此时，你需要选定一个数字 X，使我们可以将整副牌按下述规则分成 1 组或更多组：

每组都有 X 张牌。

组内所有的牌上都写着相同的整数。

仅当你可选的 X >= 2 时返回 true。

<!--more-->

示例 1：

``` javascript
输入：[1,2,3,4,4,3,2,1]
输出：true
解释：可行的分组是 [1,1]，[2,2]，[3,3]，[4,4]
```
示例 2：

```
输入：[1,1,1,2,2,2,3,3]
输出：false
解释：没有满足要求的分组。
```
示例 3：

``` javascript
输入：[1]
输出：false
解释：没有满足要求的分组。
```
示例 4：

``` javascript
输入：[1,1]
输出：true
解释：可行的分组是 [1,1]
```
示例 5：

``` javascript
输入：[1,1,2,2,2,2]
输出：true
解释：可行的分组是 [1,1]，[2,2]，[2,2]
```
题解

``` javascript
var hasGroupsSizeX = function(deck) {
    function gcd(a, b) {
        if (a % b === 0) {
            return b;
        }
        let c = b
        let d = a % b
        return gcd(c,d)
    }
    let deckLength = deck.length
    let noRepetArr = [...new Set(deck)]
    let noRepetArrLength = noRepetArr.length
    let minDivisor = 0
    if (deckLength === 1) return false
    for (let i = 0; i<noRepetArrLength; i++) {
        let itemArr = deck.filter((item)=>{
            return item === noRepetArr[i]
        })
        let itemMinDivisor = itemArr.length
        if (i === 0) {
            minDivisor = itemMinDivisor
        } else {
            minDivisor = gcd(minDivisor,itemMinDivisor)
        }
    }
    if (minDivisor === 1) {
        return false
    }
    return true
};
```
使用辗转相除法 递归方式算最大公因数

``` javascript
    function gcd(a, b) {
        if (a % b === 0) {
            return b;
        }
        let c = b
        let d = a % b
        return gcd(c,d)
    }
```