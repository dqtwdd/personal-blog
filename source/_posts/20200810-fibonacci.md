---
title: 动态规划实现斐波那契数列
tags: ['算法']
categories: 算法
date: 2020-08-10
---
如果是index是0，1，2直接返回对应值（使用空间换时间）

如果index>2，使用动态规划遍历一次获得需要的值。

<!--more-->

``` javascript
// 动态规划
function fib(n) {
    if (n === 0) return 0
    if (n === 1 || n===2) return 1
    let prev = 1
    let curr = 1
    for (let i=3;i<=n;i++) {
        let sum = curr + prev
        prev = curr
        curr = sum
    }
    return curr
}
fib(0)//0
fib(1)//1
fib(2)//1
fib(3)//2
fib(4)//3
fib(5)//5
```