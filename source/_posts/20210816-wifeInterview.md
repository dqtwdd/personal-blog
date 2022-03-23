---
title: 董大宝的面试算法总结
tags: ['算法']
categories: 算法
date: 2021-08-16
---

老婆面试的算法题，总结一下。

<!--more-->

### 1. 实现一个千分位，扩展string的原型：这个可以有多种思路，不用反转。

本题考查点：

1. 原型上的this指向本身。

2. 原型和原型链，在原型上拓展方法。

3. 带有小数点时，要只从整数位开始加分号。

核心思路：

1. 将字符串分为整数和小数两部分。
2. 将整数反向遍历，遍历到序号是3的整数倍并且序号不为0时加一个','。需要注意的是反向遍历时可以写作 newStr = xxx+newStr（取代 newStr += xxx ），将反转的步骤就给省略了。
3. 遍历完成后加上小数部分，返回。

#### 1.1 使用`toLocaleString`方法。

```javascript
String.prototype.thousands = function () {
  if (this * 1 === NaN) {
    return NaN;
  }
  if (this.length <= 3) {
    return this.toString();
  }
  let num = this * 1;
  return num.toLocaleString();
};
let str = '12345678';
console.log(str.thousands()); // '12,345,678'
```

#### 1.2 将字符串反转，加逗号后再次反转并返回

```javascript
String.prototype.thousands = function () {
  if (this * 1 === NaN) {
    return NaN;
  }
  if (this.length <= 3) {
    return this.toString();
  }
  let reverseStr = '';
  let intNum = this.split('.')[0];
  let index = 0;
  for (let i = intNum.length - 1; i >= 0; i--) {
    reverseStr += intNum[i];
    index += 1;
    if (index % 3 === 0) {
      reverseStr += ',';
    }
  }
  let resStr = '';
  for (let i = reverseStr.length - 1; i >= 0; i--) {
    resStr += reverseStr[i];
  }
  resStr += this.split('.')[1] === undefined ? '' : '.' + this.split('.')[1];
  return resStr;
};
let str = '12345678.635636';
console.log(str.thousands()); // 12,345,678.635636
```

#### 1.3 优化不反转，直接处理

```javascript
String.prototype.thousands = function () {
  let str = this;
  let intStr = str.split('.')[0];
  let newStr = '';
  let reverseIndex = 0;
  for (let i = intStr.length - 1; i >= 0; i--) {
    newStr = intStr[i] + newStr;
    reverseIndex += 1;
    if (reverseIndex % 3 === 0 && reverseIndex !== 0) {
      newStr = ',' + newStr;
    }
  }
  return newStr + (str.split('.')[1] ? '.' + str.split('.')[1] : '');
};
let str = '12';
console.log(str.thousands()); // 12
```

#### 1.4 正则

```javascript
String.prototype.thousands = function () {
  let res = this.toString().replace(/\d+/, function (num) {
    return num.replace(/(\d)(?=(\d{3})+$)/g, function ($1) {
      return $1 + ',';
    });
  });
  return res;
};
let str = '12345.678';
console.log(str.thousands()); // 12,345.678
```

### 2. 实现字符串的全排列

本题考查点：回溯算法。

回溯算法就相当于树的遍历，网上关于回溯的讲解有很多，贴一篇个人感觉比较好的：

[带你学透回溯算法！47. 全排列 II](https://leetcode-cn.com/problems/permutations-ii/solution/dai-ma-sui-xiang-lu-dai-ni-xue-tou-hui-s-ki1h/)

回溯很简单，记住一串伪代码

```javascript
function backtracking(参数) {
    if (终止条件) {
        存放结果;
        return;
    }

    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
      	if (跳过此循环条件) {
          continue;
        }
        处理节点;
        backtracking(路径，选择列表); // 递归
        回溯，撤销处理结果
    }
}
```

对于一些稍微复杂的回溯问题，需要剪枝，就是要去除重复数据，去除重复数据有两种办法，一种是将走过的路径记录，另一种是遍历完成后将结果暴力去重。

#### 2.1 简单回溯

简单回溯问题包括：无重复字符串的全排列，无重复元素数组的全排列等。

```javascript
/**
 * 全排列
 * @param {number[]} nums
 * @return {number[][]}
 */
var permute = function (nums) {
  let currPath = [];
  let res = [];
  backtrack(nums, []);
  return res;
  function backtrack(nums, used) {
    if (currPath.length === nums.length) {
      res.push(JSON.parse(JSON.stringify(currPath)));
      return;
    }
    for (let i = 0; i < nums.length; i++) {
      if (used[i]) {
        continue;
      }
      currPath.push(nums[i]);
      used[i] = true;
      backtrack(nums, used);
      currPath.pop();
      used[i] = false;
    }
  }
};

```

#### 2.2 回溯&剪枝

回溯剪枝问题包括：包含重复字符的全排列，包含重复元素的数组全排列。

```javascript

/**
 * 使用剪枝条件判断
 * used[i - 1] == true，说明同一树支nums[i - 1]使用过
 * used[i - 1] == false，说明同一树层nums[i - 1]使用过
 * 如果同一树层nums[i - 1]使用过则直接跳过
 *
 * @param {number[]} nums
 * @return {number[][]}
 */
var permuteUnique = function (nums) {
  let res = [];
  let path = [];
  let sortNums = nums.sort((a, b) => a - b);
  backtrack(sortNums, []);
  return res;

  function backtrack(nums, used) {
    if (path.length === nums.length) {
      res.push(JSON.parse(JSON.stringify(path)));
      return;
    } // 满足条件时返回
    for (let i = 0; i < nums.length; i++) {
      if (used[i] || (i > 0 && nums[i] === nums[i - 1] && !used[i - 1])) {
        continue;
      } // 使用过或满足剪枝条件时剪枝
      path.push(nums[i]); // 做选择
      used[i] = true;
      backtrack(nums, used); // 递归继续做选择
      path.pop(); // 回溯
      used[i] = false; // 回溯
    }
  }
};

/**
 * 先全排列，得到结果后暴力去重
 *
 * @param {number[]} nums
 * @return {number[][]}
 */

var permuteUniqueWithSet = function (nums) {
  let res = [];
  let path = [];
  backtrack(nums, []);

  return Array.from(new Set(res)).map((item) => {
    return item.split('').map((sonItem) => {
      return sonItem * 1;
    });
  });

  function backtrack(nums, used) {
    if (path.length === nums.length) {
      res.push(JSON.parse(JSON.stringify(path.join(''))));
      return;
    }
    for (let i = 0; i < nums.length; i++) {
      if (used[i]) {
        continue;
      }
      path.push(nums[i]);
      used[i] = true;
      backtrack(nums, used);
      path.pop();
      used[i] = false;
    }
  }
};

```







