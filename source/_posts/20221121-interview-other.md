---
title: 面试准备-webpack/xss/其他
tags: ['工作']
categories: 工作
date: 2022-11-21
---

记录工作中想到的面试应该会的问题。

<!--more-->

## 4.webpack 相关知识

### 4.1 webpack 与 glup 区别

webpack 是一个模块化打包工具，他可以将各种格式的文件通过 loader（加载器）和 plugin（插件）对资源进行处理，打包成符合环境部署的前端资源。webpack 强调的是模块化打包。
webpack is a module bundle.

loader : sass-loader、svg-sprite-loader 等

pugin : DllPlugin DllReferencePlugin speed-measure-webpack-plugin webpack-bundle-analyzer

glup 强调的是前端开发的工作流程，我们可以通过配置一系列的 task（比如压缩文件合并，雪碧图，版本控制等）然后定义执行顺序，然后让 glup 执行这些 task，从而构建整个项目。glup 强调的是开发流程。
glup is a task Runner.

### 4.2 webpack 原理

webpack 打包原理：当 webpack 处理程序时，会根据文件间的依赖关系对其进行静态分析，根据依赖关系递归的构建一个依赖关系图（dependency graph），然后将这些模块按照指定的规则生成静态资源，其中包含应用程序需要的每个模块，然后将这些模块打包程一个或多个 bundle。

## 5. XSS

## 6.数据结构

### 6.1 二叉树

二叉树是一种典型的树状结构，有根、左子树和子右子树。

前序遍历 根左右 中序遍历 左根右 后序遍历 左右根

只要有第一个，就一直找第一个。

下图中 前序：ABCDEGF 中序：CBEGDFA 后序：CGEFDBA

![BinaryTree](https://dqtwdd.top/cdn/img/BinaryTree.png)

### 6.2 链表

链表的就是用一组任意储存单元来储存线性表的数据结构，每一个节点对象储存着它本身的值和他指向的下一个节点的地址。

链表分 线性链表，双向链表，环形链表。

## 7. 算法

### 7.1 数组扁平化

### 7.2 动态规划

### 7.3 获取地址栏中所有参数

## 8. 有哪些项目优化方法

### 8.1 加快 HTML 页面加载速度

1. 页面减肥
   
   - 删除不必要的空格、注释。
   - 将 inline 的 script 和 css 移到外部文件。

2. 减少文件数量
   
   - 减少页面上引用的文件数量可以减少 HTTP 链接数。
   - 许多 JavaScript、CSS 文件可以合并最好合并。

3. 减少域名查询
   
   DNS 查询和解析也是消耗时间的，所以要减少对外部 JavaScript、CSS 图片等资源的引用，不同域名的使用越少越好。

4. 缓冲重用数据
   
   - 对重复使用的数据进行缓存

5. 优化页面元素的加载顺序
   
   - 首先加兹安页面最初显示的内容与之相关的 JavaScript 和 CSS
   - 然后加载 HTML 相关的东西，像什么不是最初显示相关的图片，视频等很肥的资源就最后加加载。

6. 指定图像和 table 的大小
   
   如果浏览器可以立即决定图像或者 table 的大小，那么他就可以马上显示而不需要重新做一些布局安排工作。这不仅加快了页面显示，也预防了页面完成加载后布局的一些不当的改变。

## 9. 工作中遇到的兼容性问题

## 10. 浏览器相关

### 10.1 浏览器缓存机制
