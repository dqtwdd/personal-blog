---
title: 算法
tags: ["学习"]
categories: 学习
date: 2022-06-16
---

总结一下面试准备的算法和所用时间。

<!--more-->

### 1. [岛屿问题](https://leetcode.cn/problems/number-of-islands/)

```javascript
/**
 * @param {character[][]} grid
 * @return {number}
 * time:2022/06/16-16:47
 * total:30min
 */
/**
 * @param {character[][]} grid
 * @return {number}
 * time:2022/06/16-16:47
 * total:30min
 */

var numIslands = function (grid) {
  // 淹没
  let res = 0;
  function submerged(i, j) {
    grid[i][j] = '0';

    if (grid[i + 1] && grid[i + 1][j] === '1') submerged(i + 1, j);
    if (grid[i - 1] && grid[i - 1][j] === '1') submerged(i - 1, j);
    if (grid[i][j + 1] === '1') submerged(i, j + 1);
    if (grid[i][j - 1] === '1') submerged(i, j - 1);
  }
  for (let i = 0; i < grid.length; i++) {
    for (let j = 0; j < grid[i].length; j++) {
      if (grid[i][j] === '1') {
        res += 1;
        submerged(i, j);
        console.log('grid:', grid);
      }
    }
  }
  return res;
};
```

### 2. [最长回文子串](https://leetcode.cn/problems/longest-palindromic-substring/submissions/)

```javascript
var longestPalindrome = function (s) {
  let res = '';
  let getMax = function (s, l, r) {
    while (l >= 0 && r < s.length && s[l] === s[r]) {
      l--;
      r++;
    }
    return s.slice(l + 1, r);
  };
  for (let i = 0; i < s.length; i++) {
    let a = getMax(s, i, i);
    let b = getMax(s, i, i + 1);
    res = a.length > res.length ? a : res;
    res = b.length > res.length ? b : res;
  }
  return res;
};
```

### 3. [接雨水](https://leetcode.cn/problems/trapping-rain-water)

```javascript
// 核心思想 中心扩散。找到最低点，然后根据最低点算出当前水坑水的体积，然后再找下一个水坑
/**
 * @param {number[]} height
 * time:2022/06/17 10:13 gg
 * time:2022/06/17 11:05 中心扩散 15:20完成 我是菜逼
 * @return {number}
 */
var trap = function (height) {
  if (height.length < 3) return 0;
  let res = 0;
  function getWater(mid) {
    let l = mid - 1;
    let r = mid + 1;
    let lM = l;
    let rM = r;
    while (l >= 0 && r < height.length) {
      if (height[lM] >= height[rM]) {
        r++;
        rM = height[r] > height[rM] ? r : rM;
      } else {
        l--;
        lM = height[l] > height[lM] ? l : lM;
      }
    }
    let min = Math.min(height[rM], height[lM]);
    let tempRes = min * (rM - 1 - lM);
    for (let i = lM + 1; i < rM; i++) {
      tempRes -= height[i];
    }
    return {
      res: tempRes,
      r: rM,
    };
  } // 从最低点中心扩散取水
  for (let i = 1; i < height.length - 1; ) {
    if (height[i - 1] >= height[i] && height[i + 1] >= height[i]) {
      let funcRes = getWater(i);
      res += funcRes.res;
      i = funcRes.r;
    } else {
      i++;
    }
  } // 找最低点
  return res;
};
```

### 4.[生成括号](https://leetcode.cn/problems/generate-parentheses/submissions/)

```javascript
/**
 * @param {number} n
 * time:2020-06-17 16:23 回溯
 * 10min 我是小天才
 * 回溯需要注意的就是两点：
 * 1.停止条件
 * 2.继续进行的条件
 * @return {string[]}
 */
var generateParenthesis = function (n) {
  let res = [];
  function dfs(l, r, s) {
    if (s.length === 2 * n) {
      res.push(s);
      return;
    }
    if (l > 0) dfs(l - 1, r, s + '(');
    if (l < r) dfs(l, r - 1, s + ')');
  }
  dfs(n, n, '');
  return res;
};

let n = 3;

console.log(generateParenthesis(3));
```

### 5. [爬楼梯](https://leetcode.cn/problems/climbing-stairs/)

```javascript
/**
 * time:2022-06-17 16:36
 * 16:48 12min
 * 思路：
 * 这道题应该动态规划和回溯都能做，本次尝试用回溯做。
 * 还是两个注意点：
 * 1.终止条件
 * 2.进行条件
 * @param {number} n
 * @return {number}
 */
var climbStairs = function (n) {
  let res = [];
  function go(stairs, gone) {
    if (stairs === 0) {
      res.push(gone);
      return;
    }
    if (stairs >= 2) {
      go(stairs - 1, gone + '1');
      go(stairs - 2, gone + '2');
    } else {
      go(stairs - 1, gone + '1');
    }
  }
  go(n, '');
  return res.length;
};

console.log(climbStairs(35)); // 14930352 虽然很慢，但是确实对了。

/**
 * time:2022-06-17 16:49
 * 回溯太变态了，耗时极长，因此用动态规划再做一遍
 * 动态规划思路：
 * 后面的跟前面的有关系，先找这个关系，一般都是 a = a-1 + a-2这种。
 * @param {number} n
 * @return {number}
 */
var climbStairs = function (n) {
  if (n === 1) return 1;
  if (n === 2) return 2;
  let res = [1, 2];
  for (let i = 2; i < n; i++) {
    res[i] = res[i - 1] + res[i - 2];
  }
  return res[n - 1];
};

console.log(climbStairs(35)); // 14930352
```

然后举一反三搞了一个暴力破解密码的小脚本

```javascript
// 暴力破解六位密码;
function force(pos) {
  let res = [];
  let passDic = [
    'q',
    'w',
    'e',
    'r',
    't',
    'y',
    'u',
    'i',
    'o',
    'p',
    'a',
    's',
    'd',
    'f',
    'g',
    'h',
    'j',
    'k',
    'l',
    'z',
    'x',
    'c',
    'v',
    'b',
    'n',
    'm',
    '0',
    '1',
    '2',
    '3',
    '4',
    '5',
    '6',
    '7',
    '8',
    '9',
  ];
  function getPass(left, str) {
    if (left === 0) {
      // console.log(str);
      res.push(str);
      return;
    }
    for (let i = 0; i < passDic.length; i++) {
      getPass(left - 1, str + passDic[i]);
    }
  }
  getPass(pos, '');
  return res;
}
let timeStart = Date.parse(new Date());
console.log(timeStart);
let passList = force(5);
let timeEnd = Date.parse(new Date());
console.log(timeEnd.toString() * 1 - timeStart.toString() * 1);
```

### 6. [买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock)

```javascript
/**
 * @param {number[]} prices
 * time: 16:45:00 26min
 * @return {number}
 */
// 最弱智的双层遍历
var maxProfit = function (prices) {
  let res = 0;
  for (let i = 0; i < prices.length - 1; i++) {
    let curr = prices[i];
    for (let j = i + 1; j < prices.length; j++) {
      if (prices[j] <= prices[i]) continue;
      let profit = prices[j] - prices[i];
      res = profit > res ? profit : res;
    }
  }
  return res;
};
console.log(maxProfit([7, 1, 5, 3, 6, 4]));
/**
 * 剪枝
 * 如果当前值小于最小买入值直接继续
 * 如果当前值小于买入值直接继续
 */
var maxProfit = function (prices) {
  let res = 0;
  let minBuy = prices[0];
  for (let i = 0; i < prices.length - 1; i++) {
    let curr = prices[i];
    if (curr > minBuy) continue;
    for (let j = i + 1; j < prices.length; j++) {
      if (prices[j] <= prices[i]) continue;
      let profit = prices[j] - prices[i];
      if (profit > res) {
        minBuy = curr;
        res = profit;
      }
    }
  }
  return res;
};
console.log(maxProfit([1, 2]));
// 最好的办法
// 如果当前值小于最小值 直接赋给最小值 然后继续
// 如果不大于最小值，算利润，取比较高的利润
var maxProfit = function (prices) {
  let min = prices[0];
  let maxProfit = 0;
  for (i = 0; i < prices.length; i++) {
    if (prices[i] < min) {
      min = prices[i];
    } else {
      maxProfit = Math.max(maxProfit, prices[i] - min);
    }
  }
  return maxProfit;
};
console.log(maxProfit([7, 1, 5, 3, 6, 4]));
```

### 7.[最大子数组和](https://leetcode.cn/problems/maximum-subarray/) / [连续子数组的最大和](https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/)

这两道题解法基本一致。

动态规划的步骤：

1. 穷举（举出部分结果，寻找dp[i]和dp[i-1]的规律）
2. 确定边界（就是找特殊的例子，相当于剪支）
3. 找出转移态方程（寻找dp[i]和dp[i-1]的关系）
4. 求解

```javascript
/**
 * @param {number[]} nums
 * time:2022-07-01 15:56 16:12
 * @return {number}
 */
// dp[i+1] = Math.max(dp[i]+nums[i+1],nums[i+1])
var maxSubArray = function (nums) {
  if (nums.length < 2) {
    return nums.length === 0 ? 0 : nums[0];
  }
  let max = nums[0];
  let dp = [nums[0]];
  let i = 1;
  while (i < nums.length) {
    dp[i] = Math.max(dp[i - 1] + nums[i], nums[i]);
    max = Math.max(dp[i], max);
    i++;
  }
  return max;
};

let nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]; // 6
console.log(maxSubArray(nums));
```

### 8. [跳跃游戏 II](https://leetcode.cn/problems/jump-game-ii/)

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 * time:14:00
 * 通过审题得出转移态方程
 * dp[i] = dp[i-1] + nums[i]
 * 需要注意的是不一定直接跳到最后一个台阶，可能跳的更远，所以需要记录一下中间的部分。防止有empty
 */
var jump = function (nums) {
  if (nums.length <= 2) {
    return nums.length === 2 ? 1 : 0;
  }
  let dp = [0, 1];
  for (let i = 0; i < nums.length; i++) {
    for (let j = i + 1; j <= i + nums[i]; j++) {
      if (dp[j] && dp[i]) {
        dp[j] = Math.min(dp[j], dp[i] + 1);
      } else {
        if (dp[i] !== undefined) {
          dp[j] = dp[i] + 1;
        }
      }
    }
  }
  return dp[nums.length - 1];
};
```

### 9.[斐波那契数列](https://leetcode.cn/problems/fei-bo-na-qi-shu-lie-lcof/)

```javascript
/**
 * 最简单的动态规划注意下取模就行
 * @param {number} n
 * @return {number}
 * time:11:08
 * dp[n] = dp[n-1]+dp[n-2]
 */
var fib = function (n) {
  let dp = [0, 1];
  if (n < 2) return dp[n];
  for (let i = 2; i <= n; i++) {
    dp[i] = dp[i - 1] + dp[i - 2];
    dp[i] = ((dp[i] % 1000000007) + 1000000007) % 1000000007;
  }
  return dp[n];
};
console.log(fib(81));

```

### 9.[打家劫舍](https://leetcode.cn/problems/house-robber/)

```javascript
/**
 * @param {number[]} nums
 * time:14:25 15min
 * 打家劫舍的dp模型还是比较简单的
 * dp[i] = Math.max(dp[i-2],dp[i-3]) + nums[i]
 * @return {number}
 */
var rob = function (nums) {
  if (nums.length === 0) return 0;
  if (nums.length === 1) return nums[0];
  if (nums.length === 2) return Math.max(nums[0], nums[1]);
  if (nums.length === 3) return Math.max(nums[0] + nums[2], nums[1]);
  let dp = [
    nums[0],
    Math.max(nums[0], nums[1]),
    Math.max(nums[0] + nums[2], nums[1]),
  ];
  let max = Math.max(...dp);

  for (let i = 3; i < nums.length; i++) {
    dp[i] = Math.max(dp[i - 2], dp[i - 3]) + nums[i];
    if (dp[i] > max) max = dp[i];
  }
  return max;
};
let nums = [2, 8, 1, 3, 5]; // 12
console.log(rob(nums));
```

