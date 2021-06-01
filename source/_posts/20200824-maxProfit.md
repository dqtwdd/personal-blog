---
title: 买卖股票的最佳时机
tags: ['算法','贪心算法']
categories: 算法
date: 2020-08-24
---

给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。

注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

 <!--more-->

示例 1:

输入: [7,1,5,3,6,4]
输出: 7
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 。
示例 2:

输入: [1,2,3,4,5]
输出: 4
解释: 在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。
     因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。
示例 3:

输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。

这道题么，先是用自己脑子整理了一下思路，其实以后遇到比较困难的题没有好的解决方案就可以按照接下来的流程去做，首先写根据题目写一个思路的拓扑图，然后整理一下，直接按照拓扑图的思路写就完了，优点是思路清晰，可以很容易的得出答案，缺点是这样得出来的答案一般都不是最优解，都是比较笨的办法。

思维拓扑图：

![解法拓扑图](https://s1.ax1x.com/2020/08/24/drZb5t.png)

然后按部就班，按照每一个节点写代码就完了

```javascript
/**
 * @param {number[]} prices
 * @return {number}
 */
var maxProfit = function(prices) {
  let profitSum = 0
  let buyPrice
  for (let i = 0; i < prices.length; i++) {
    if (buyPrice !== undefined) {
      if (!prices[i+1]) {
        profitSum = profitSum + prices[i] - buyPrice
        buyPrice = undefined
      } else {
        if (prices[i] > prices[i+1]) {
          profitSum = profitSum + prices[i] - buyPrice
          buyPrice = undefined
        }
      }
    } else {
      if (prices[i+1] === undefined) {
        return profitSum
      } else {
        if (prices[i] < prices[i+1]) {
          buyPrice = prices[i]
        }
      }
    }
    console.log(buyPrice,profitSum)
  }
  return profitSum
};
```
结果嘛emmm...不出所料的慢。

这里还有一个小插曲，在判断时要考虑清楚判断的对象范围，比如本题中

`if (buyPrice !== undefined)`

我一开始写的就是

`if (!buyPrice)`

**导致当buyPrice为0时直接跳过了，所以一定要考虑清楚判断语句中的内容！非常重要**

接下来就应该思考怎么优化，这道题就使用贪心算法，如果右边的比左边大，就直接的出来当前的利润加进去就好。

根据这个思路优化后，果然快了很多哦~

```javascript
/**
 * @param {number[]} prices
 * @return {number}
 */
var maxProfit = function(prices) {
    let profitSum = 0
    for (let i = 0; i < prices.length;i++) {
      if (prices[i] === undefined) return
      if (prices[i] < prices[i+1]) {
        profitSum = profitSum + prices[i+1] - prices[i]
      }
    }
    return profitSum
  };
let arr = [1,2,3,4,5]
console.log(maxProfit(arr))
```
