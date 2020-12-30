---
title: 最多 K 次交换相邻数位后得到的最小整数
tags: ['算法', '贪心算法']
categories: 算法
---

给你一个字符串  num 和一个整数  k 。其中，num 表示一个很大的整数，字符串中的每个字符依次对应整数上的各个 数位 。

你可以交换这个整数相邻数位的数字 最多  k  次。

请你返回你能得到的最小整数，并以字符串形式返回。

<!--more-->

示例 1：
输入：num = "4321", k = 4
输出："1342"
解释：4321 通过 4 次交换相邻数位得到最小整数的步骤如上图所示。

示例 2：
输入：num = "100", k = 1
输出："010"
解释：输出可以包含前导 0 ，但输入保证不会有前导 0 。

示例 3：
输入：num = "36789", k = 1000
输出："36789"
解释：不需要做任何交换。

示例 4：
输入：num = "22", k = 22
输出："22"

示例 5：
输入：num = "9438957234785635408", k = 23
输出："0345989723478563548"

提示：
1 <= num.length <= 30000
num  只包含   数字   且不含有   前导 0 。
1 <= k <= 10^9

来了来了！力扣征途中第一道困难题！出现了！！！！！

虽然没跑过。哈哈哈。

测试用例太大了，但是思路和方法应该没问题，所差的就是不是最优解，因为不太了解树状数组结构，以后学习一哈。

核心思想就是双指针，然后遍历一遍数组将区间内的最小值挪到前面就可以了。

虽然没跑过，但是还是要实名表扬一下自己。因为：

1.没用双层 for 循环。只遍历了一次。

2.代码比题解短。

3.思路清晰，养成了画 UML 图的好习惯。

4.学以致用，感觉最近做题时看到的视窗思想、双指针思想、和贪心算法的知识都用上了。

鼓掌！

![解法拓扑图](https://s1.ax1x.com/2020/09/01/dxkMlj.md.png)

```javascript
/**
 * @param {string} num
 * @param {number} k
 * @return {string}
 */
var minInteger = function (num, k) {
  let numArr = num.split('') // 将num转化为数组
  // 获取右侧指针位置，由于arr.slice(i,j)方法截取的是从i位置到j位置之前的数组，所以将右侧指针+1
  for (let i = 0; i < numArr.length; i++) {
    let r = 0
    if (k > numArr.length - i) {
      r = numArr.length + 1
    } else {
      r = i + k + 1
    }
    // console.log('i:', i, 'r:', r, 'numArr.slice(i, r):', numArr.slice(i, r))
    // 获取当前视窗之内的数组
    let newArr = numArr.slice(i, r)
    // 获取当前视窗数组之内的最小值
    let min = Math.min(...newArr) + ''
    // 获取当前视窗数组之内的最小值的下标index，index + i为当前获取当前视窗数组之内的最小值在完整数组中的下标
    let minIndex = newArr.indexOf(min) + i
    // console.log('min:', min, 'minIndex:', minIndex)
    // 获取当前视窗数组之内的最小值
    let temp = numArr[minIndex]
    // 删除数组中的当前视窗数组之内的最小值
    numArr.splice(minIndex, 1)
    // 将当前视窗数组之内的最小值插入到右侧指针处
    numArr.splice(i, 0, temp)
    // 更新k值
    k = k - (minIndex - i)
    // console.log('numArr:', numArr, 'k:', k)
    // 当k为0时返回
    if (k === 0) return numArr.join('')
  }
  // 遍历完成后返回
  return numArr.join('')
}

let num = '51423'
let k = 4
console.log(minInteger(num, k))
```

```javascript
// 更新后的算法。以后尽量不要操作数组。
var minInteger = function (num, k) {
  // 获取最大循环次数
  let maxExchange = 0
  for (let i = 1; i < num.length; i++) {
    maxExchange += i
  }
  k = Math.min(k, maxExchange)
  for (let i = 0; i < num.length; i++) {
    let r = 0
    if (k > num.length - i) {
      r = num.length + 1
    } else {
      r = i + k + 1
    }
    let newArr = num.substring(i, r)

    // 获取当前视窗内数组的最小值，做题时拒绝使用Math方法
    let min = newArr[0]
    let minIndex = 0
    for (let j = 0; j < newArr.length; j++) {
      let cur = newArr[j]
      cur < min ? ((min = cur), (minIndex = j + i)) : null
    }
    // 增加判断，只有当前最小值的位置坐标在右指针后面时才执行里面的代码
    if (minIndex > i) {
      // 之前没跑过的原因就在这里，原来的做法是操作数组，最后再合并成字符串，这样在测试用例很大时非常吃内存，也给我提了一个醒，能操作字符串就不要去操作数组。
      num =
        num.substring(0, i) +
        num[minIndex] +
        num.substring(i, minIndex) +
        num.substring(minIndex + 1)
      k = k - (minIndex - i)
    }
    if (k === 0) return num
  }
  return num
}
```
