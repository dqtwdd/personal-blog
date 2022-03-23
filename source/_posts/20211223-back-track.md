---
title: 回溯算法合集
tags: ['算法','回溯']
categories: 算法
date: 2021-12-23
---

回溯算法合集。

<!--more-->

### 回溯算法模版

```javascript
function backtrack (list,used) {
  if (判断保存结果条件) {
    保存结果
    return
  }
  for (横向遍历) {
    if (判断条件) {剪枝}
    处理节点
    backtrack() // 递归 深度遍历
    回溯撤销处理结果
  }
}
```



### 电话号码的字母组合

给定一个仅包含数字 `2-9` 的字符串，返回所有它能表示的字母组合。答案可以按 **任意顺序** 返回。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/11/09/200px-telephone-keypad2svg.png)

 

**示例 1：**

```
输入：digits = "23"
输出：["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

**示例 2：**

```
输入：digits = ""
输出：[]
```

**示例 3：**

```
输入：digits = "2"
输出：["a","b","c"]
```

 **提示：**

- `0 <= digits.length <= 4`
- `digits[i]` 是范围 `['2', '9']` 的一个数字。

```javascript
/**
 * @param {string} digits
 * @return {string[]}
 */
 var letterCombinations = function (digits) {
  if (digits.length < 1) return [];
  let numLetterList = {
    2: ['a', 'b', 'c'],
    3: ['d', 'e', 'f'],
    4: ['g', 'h', 'i'],
    5: ['j', 'k', 'l'],
    6: ['m', 'n', 'o'],
    7: ['p', 'q', 'r', 's'],
    8: ['t', 'u', 'v'],
    9: ['w', 'x', 'y', 'z'],
  };
  resList = [];
  digits = digits.split('');
  digits.map((item) => {
    resList.push(numLetterList[item]);
  });
  let path = [];
  let res = [];
  function backTrack(list, used) {
    if (path.length === resList.length) {
      res.push(JSON.parse(JSON.stringify(path.toString().replace(/,/g, ''))));
      return;
    }
    for (let i = 0; i < list.length; i++) {
      if (used > i) continue;
      for (let j = 0; j < list[i].length; j++) {
        path.push(list[i][j]);
        i++;
        backTrack(list, i);
        i--;
        path.pop();
      }
    }
  }
  backTrack(resList, 0);
  return res;
};
```



### 删除链表的倒数第 N 个结点

给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)

```
输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]
```

**示例 2：**

```
输入：head = [1], n = 1
输出：[]
```

**示例 3：**

```
输入：head = [1,2], n = 1
输出：[1]
```

**提示：**

- 链表中结点的数目为 `sz`
- `1 <= sz <= 30`
- `0 <= Node.val <= 100`
- `1 <= n <= sz`

**进阶：**你能尝试使用一趟扫描实现吗？

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @param {number} n
 * @return {ListNode}
 */
var removeNthFromEnd = function (head, n) {
  let start = head;
  let map = new Map();
  let index = 1;
  while (head) {
    map.set(index, head);
    head = head.next;
    index++;
  }
  let pre = map.get(index - n-1);
  let next = map.get(index - n + 1);
  if (pre&&next) {
      pre.next = next
  } else if (next) {
      start = next
  } else if (pre) {
      pre.next = null
  } else {
      start = null
  }
  return start
};
```

