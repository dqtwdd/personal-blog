---
title: 判断括号有效性---bilibili笔试题
tags: ['算法']
categories: 算法
date: 2020-08-14
---
给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
注意空字符串可被认为是有效字符串。

<!--more-->
示例 1:

```javascript
输入: "()"
输出: true
```

示例 2:
```javascript
输入: "()[]{}"
输出: true
```

示例 3:
```javascript
输入: "(]"
输出: false
```

示例 4:
```javascript
输入: "([)]"
输出: false
```

示例 5:
```javascript
输入: "{[]}"
输出: true
```

先说说我自己的想法。

首先将字符串分割成数组，将所有括号分成6种（圆括号 左 右、方括号 左 右、花括号 左 右），然后记录每种括号所在的位置，记录index，然后分三类，圆括号方括号和花括号，然后判断每种括号的左右数量是不是一致（isDouble），不一致直接返回false。

然后重点来了，数组一定是以左括号开始的，并且每种类型的第一个右括号一定和最后一个左括号配对。

配对括号中间一定要间隔偶数个位置，如果是奇数就说明中间有没配对的。根据这个再判断一次成对括号的index之差是不是奇数（isSingle）。

只有满足两个条件才返回true，否则返回false。上代码

```javascript
/**
 * @param {string} s
 * @return {boolean}
 */
var isValid = function(s) {
  let sArr = s.split('')
  console.log(sArr)
  let squareLArr = []
  let circleLArr = []
  let flowerLArr = []
  let squareRArr = []
  let circleRArr = []
  let flowerRArr = []
  for (let i = 0; i < sArr.length; i++) {
    switch (sArr[i]) {
      case '(' :
        circleLArr.push(i)
      break;
      case '[' :
        squareLArr.push(i)
      break;
      case '{' :
        flowerLArr.push(i)
      break;
      case ')' :
        circleRArr.push(i)
      break;
      case ']' :
        squareRArr.push(i)
      break;
      case '}' :
        flowerRArr.push(i)
      break;
    }
  }
  let circleArr = [circleLArr,circleRArr]
  let squareArr = [squareLArr,squareRArr]
  let flowerArr = [flowerLArr,flowerRArr]
  let isDouble = double(circleArr) && double(squareArr) && double(flowerArr)
  console.log(isDouble)
  let isSingle  = single(circleArr) && single(squareArr) && single(flowerArr)
  console.log(isSingle)
  if (isDouble && isSingle) {
    return true
  }
  return false
};
function double(b) {
  return b[0].length === b[1].length
}
function single(b) {
  for (let i = b[0].length - 1; i >= 0; i--) {
    for (let j = 0; j < b[1].length; j++) {
      if (b[0][i] < b[1][j]) {
        let gap = b[1][j] - b[0][i]
        if (gap % 2  === 0) {
          console.log(b,i,j,b[1][j],b[0][i])
          return false
        }
        b[0].splice(i,1)
        b[1].splice(j,1)
        break
      }
    }
  }
  return true
}
let s1 = '[]{}(){}[]({[()]})'
isValid(s1)
```

是不是感觉我好蠢。哈哈哈哈哈哈

太复杂了。看题解发现一个野生大手子，虽然执行效率不高，但是思路很棒！

总体思路是，如果string是有效括号组，那么一定存在一对括号是左右相邻的。

如果将这对左右相邻的括号取出，那么一定会出现另一对左右相邻的括号，然后再取出来，反复执行之后，一定会将所有括号取空。

如果最后string的长度不是0，那么不是有效括号串。上代码

```javascript
/**
 * @param {string} s
 * @return {boolean}
 */
var isValid = function(s) {
  let sStr = s
  while (sStr.length > 0) {
    if (sStr.includes('()')) {
      sStr = sStr.replace('()','')
    } else if (sStr.includes('[]')) {
      sStr = sStr.replace('[]','')
    } else if (sStr.includes('{}')) {
      sStr = sStr.replace('{}','')
    } else {
      return false
    }
  }
  return true
};
```

不得不说一句，妙啊！！！

然后是官方的解法，使用栈，每出现一个左括号，将左括号堆入栈中，当出现一个右括号后，判断是否和栈中最后一个匹配，如果匹配，将最后一个括号弹出，然后继续，如果不匹配，直接返回false。

这个应该是执行效率最高的方法。

```javascript
var isValid = function(s) {
  let sArr = s.split('')
  if (sArr.length % 2 > 0) return false
  let stack = []
  for (let i = 0; i < sArr.length; i++) {
    if (sArr[i] === '(' || sArr[i] === '[' || sArr[i] === '{') {
      stack.push(sArr[i])
    } else {
      if (sArr[i] === ')') {
        if (stack[stack.length - 1] === '(') {
          stack.pop()
        }
      } else if (sArr[i] === ']') {
        if (stack[stack.length - 1] === '[') {
          stack.pop()
        }
      } else if (sArr[i] === '}') {
        if (stack[stack.length - 1] === '{') {
          stack.pop()
        }
      } else {
        console.log (i,sArr[i])
        return false
      }
    }
  }
  return !stack.length
};
```

OK!此题完结撒花！写了三种算法，我可真是棒棒哒！
