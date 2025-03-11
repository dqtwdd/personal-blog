---
title: 问题总结
tags: ['学习']
categories: 学习
date: 2025-02-07
---

### 1. 函数柯里化

```javascript
const curry = (fn, ...args) => {
  return args.length >= fn.length ? fn(...args) : (...moreArgs) => curry(fn, ...args, ...moreArgs);
};
// fn.length 代表函数fn中形式参数个数，如let add = (a,b)=>a+b 中a.length = 2;let add2=(a,b=1)=>a+b add2.length = 1 形参不能放在实参后面
```

### 2. 事件循环执行机制 、微任务宏任务

调用栈：执行代码。

任务队列：

1. 微任务。
   调用栈代码执行完成，先执行微任务队列中的代码，微任务有`promise.then async await //promise回调`、`MutationObserver //监听dom变化`、`queueMicotask //往微任务主动推送任务`、`process.nextTick //nodejs中的方法`
2. 宏任务。
   宏任务有` settimeout setinterval setimmediate requestanimationframe io操作 用户操作（click/scroll等）`
   宏任务中的宏任务放到最后。

页面渲染发生在微任务队列执行完成/一个宏任务执行完成。

**事件循环流程**

1. 执行调用栈中代码。
2. 依次取出微任务队列中的任务，依次执行。
3. 取出宏任务中的任务，然后执行，再依此清空微任务队列。
4. 重复，直至所有宏任务清空。

### 3. Vue react 解决线程阻塞的方式

1. 使用异步组件和代码分割
2. 使用watch/computed避免重复运算。
3. 使用promise
4. 使用虚拟滚动减少dom
5. 合理使用v-if

### 4. promise 

***promise不可以被取消***

promise所有静态方法：

1. `Promise.resolve`传入一个值，返回一个promise。
2. `Promise.reject`传入一个失败原因。
3. `Promise.all`传入一个promise数组，所有promise执行完成后返回结果列表，有一个失败就返回失败。
4. `Promise.any`传入一个promise数组，有一个成功就返回成功的结果，全失败的话返回失败原因的数组。
5. `Promise.race`传入一个promis数组，有一个无论成功/失败的结果，立即返回。
6. `Promise.allSettled`传入一个promise 数组，全部完成后返回成功的结果/失败的原因。

### 5. cdn 优化原理

cdn就是将网站内容（如静态文件/图片/视频等）缓存到全球各地的服务器节点，以提高家在速度和用户体验。

全球节点分布，用户就近访问。

### 6.缓存

1. Expires
   根据时客户端本地时间决定缓存时效，问题是客户端本地时间可修改，所以基本不用，改用Cache-control

2. Cache-control
   max-age **强缓存** 决定文件的缓存时长，单位是秒。

   s-maxage 当资源可以在代理服务器中缓存时，决定在代理服务器中缓存的时长，单位秒。

   no-cache **强制协商缓存** 设置了no-cache的资源会跳过强缓存校验直接去服务器进行协商缓存。

   no-store **禁止**所有缓存策略。

   public 表示资源可以在客户端被缓存，也可以被代理服务器缓存，与private互斥

   private 表示资源只能在客户端缓存，不能被代理服务器缓存，与public互斥

   must-revalidate 表示资源在过期时必须向服务器验证。（某些时候过期的资源也可能被使用，比如页面back）

### 7. 从输入网址到页面显示发生了什么

1. dns解析
   1. 先检查浏览器缓存，看是否解析过该域名。
   2. 浏览器缓存没有的话向本地DNS服务器发送请求，询问该域名对应的ip地址。
   3. 本地DNS服务器可能会有缓存该信息，或者进一步向根DNS服务器，顶级域名DNS服务器查询，直到找到对应的ip地址。

2. Tcp连接（三次握手）
3. TSL/SSL连接（https时需要）
4. 客户端发送请求
5. 服务端处理请求
6. 服务器返回响应结果
7. 客户端接收响应结果
   1. 如响应体中包含css、javascript、图片等资源引用，浏览器会继续发送请求获取资源。

8. 渲染页面
   1. 浏览器解析HTML文档，构建dom树。
   2. 浏览器加载并解析Css文件，构建cssom（Css object model）树。
   3. 将dom树与cssom树合并，生成渲染树。
   4. 根据渲染树，将浏览器进行布局。


### 8. http2 优化点

1. 多路复用
   * http/1.1：每个tcp链接只能同时处理一个请求和响应。会导致头阻塞。
   * http2：允许多个请求和响应在同一tcp连接上进行。消除了头阻塞问题，提高了带宽利用率。
2. 头部压缩
   * http/1.1：每个请求和响应都包含大量的头部信息，这些信息在多个请求之间通常是重复的，导致不必要的数据传输。
   * http2：使用HPACK算法对头部信息进行压缩，减少了传输数据量。提高了效率。
3. 服务器推送
   * http/1.1：客户端必须明确请求每个资源，服务器不能主动推送资源。
   * http2：服务器可以在客户端请求一个资源时，主动推送其他相关资源，减少了客户端的请求数量，加快了页面加载速度。
4. 二进制分帧
   * http/1.1：请求和响应是以文本形式传输的，解析效率较低。
   * http2：使用二进制分帧，将数据分割成更小的帧，提高了解析效率和传输速度。
5. 流优先级
   * http/1.1：无法控制请求的优先级，所有请求都是平等处理。
   * http2：允许为不同的流设置优先级，确保重要资源（css、javascript文件）优先加载，提高的用户体验。

### 9. https 原理

* 定义：https是http的安全版本，通过SSL/TLS协议对传输的数据进行加密，确保数据的完整性和隐私性。
* 加密技术：
  * 对称加密：通信双方使用相同的密钥进行加密解密。 缺点：密钥首次传输可能被截获，不能保证隐私。
  * 非对称加密：使用公钥和私钥配对，公钥加密的信息只能用对应的私钥解密。缺点：非对称加密算法较慢，效率较低；中间人攻击（第一次传输过程中，中间人截获双方公钥，这样就能解密双方发送的信息）。
  * 混合加密：结合两者优点，使用非对称加密交换密钥，然后使用对称加密传输数据，提高效率同事保证安全性。
* 证书机制：服务器提供由受信任的CA签发的数字证书发给客户端验证身份，防止中间人攻击。
  * 如何验证？
    证书包含两部分：1.服务端信息 2.服务端信息经hash算法后的值，用CA私钥加密后生成的数字证书。
    将服务端的信息使用hash算法得到一个hash值，记为hash1。然手使用CA的公钥解密证书中的Certificate Signature（数字签名），得到hash2，对比hash1和hash2，如果相同，则证书可信，如果不相同，则不可信。
* TSL/SSL握手过程：
  * 客户端发起请求到服务器。
  * 服务器返回证书及公钥。
  * 客户端验证证书合法性，并生成随机数作为对称加密密钥，使用服务器公钥加密后发送。
  * 服务端使用私钥解密得到对称密钥。
  * 双方使用此对称密钥加密后续通信内容。

### 10. typescript

TypeScript是JavaScript的超集，增加了静态类型检查，它允许开发者在编写代码时定义变量、函数参数和返回值的类型，从而减少运行时的错误。

* interface更适合定义对象

### 11. 项目优化

### 12. 粘性定位

### 13. 

### 14. 301 302 304

1. 1xx 信息状态码
   100 continue 继续。客户端继续请求
   101 switch protocols。切换协议。
2. 2xx 成功
   200 OK 成功 一般用于get和post
   201 Created 请求成功并且服务器创建了新的资源。
   202 Accepted 服务器已接受请求但尚未处理。
   204 No Content 没有新文档。
3. 3xx 重定向状态码
   

### 15. 抽象类

### 16. Vue 2 对于数组的监听

### 17. webpack content hash chunk hash 区别

### 18. webpack 怎么解决循环依赖

1. 延迟加载“通过`import()`动态导入语法来实现按需加载，避免一次性加载所有依赖，从而减少循环依赖的可能性，
2. 模块拆分：将大模块拆分为更小的模块，使依赖关系更加清晰，降低循环依赖的风险。
3. 重构代码：重新设计代码结构，尽量避免不必要的循环依赖。

### 19. 链表反转

### 20. 文件断点续传原理

### 21. 网页上要展示一段代码 特别的东西用标签包裹过滤

### 22. 谈谈你对前端工程化的理解

### 23. 

### 24. 岛屿数量

给你一个由 '1'（陆地）和 '0'（水）组成的的二维网格，请你计算网格中岛屿的数量。 岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。 此外，你可以假设该网格的四条边均被水包围。

```javascript
/**
 * @param {character[][]} grid
 * @return {number}
 */
var numIslands = function (grid) {
  let res = 0;
  let landDown = (x, y) => {
    grid[x][y] = 0;
    if (grid[x - 1] && grid[x - 1][y] && grid[x - 1][y] == 1) landDown(x - 1, y);
    if (grid[x + 1] && grid[x + 1][y] && grid[x + 1][y] == 1) landDown(x + 1, y);
    if (grid[x][y - 1] && grid[x][y - 1] == 1) landDown(x, y - 1);
    if (grid[x][y + 1] && grid[x][y + 1] == 1) landDown(x, y + 1);
  };
  const xMax = grid.length;
  const yMax = grid[0] && grid[0].length > 0 ? grid[0].length : 0;
  for (let i = 0; i < xMax; i++) {
    for (let j = 0; j < yMax; j++) {
      if (grid[i][j] == 1) {
        res += 1;
        landDown(i, j);
      }
    }
  }
  return res;
};
let grid = [
  ['1', '1', '1', '1', '0'],
  ['1', '1', '0', '1', '0'],
  ['1', '1', '0', '0', '0'],
  ['0', '0', '0', '0', '0'],
];
console.log(numIslands(grid));
```

### 25. LRU

```javascript
class LRUCache {
  constructor(capacity) {
    this.capacity = capacity;
    this.cache = new Map();
  }

  get(key) {
    if (!this.cache.has(key)) {
      return -1;
    }

    const value = this.cache.get(key);
    this.cache.delete(key);
    this.cache.set(key, value);

    return value;
  }

  put(key, value) {
    if (this.cache.has(key)) {
      this.cache.delete(key);
    }

    this.cache.set(key, value);

    if (this.cache.size > this.capacity) {
      const firstKey = this.cache.keys().next().value;
      this.cache.delete(firstKey);
    }
  }
}
```

