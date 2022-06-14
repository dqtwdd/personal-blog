---
title: 加水印
tags: ['工作']
categories: 工作
date: 2021-08-30
---

自己写的加水印小插件的思路。

<!--more-->

之前组内大佬写了一个加水印的小工具，对原理比较好奇，所以自己尝试写了一下。

先附上一个最后的效果。

![end](https://dqtwdd.top/cdn/img/20210903154808.png)

主要思路：

自己先思考一下，要把大象装冰箱![bingxiang](http://ww4.sinaimg.cn/large/ceeb653ejw1fbif3twmh2g203c03c74e.gif)，需要几步？

1. 打开冰箱
2. 把大象塞进去
3. 关上冰箱门

装大象一样，经过简略的思考，想到其实加水印这个简单的问题也只需要三步：

1. 在页面创建时创建一个 dom 层。（`let dom = document.createElement('div')`）
2. 将要设置为水印的文字/图片设置为 dom 背景。(`dom.style.backgroundColor = red`)
3. 将 dom 挂载在需要打水印的节点上。(`document.body.appendChild(dom);`)

So easy ～先写个小 demo：

![step1](https://dqtwdd.top/cdn/img/20210903160850.png)

![nani](http://ww1.sinaimg.cn/bmiddle/6af89bc8gw1f8q2elz9aqj205i045jr8.jpg)纳尼？我页面呢？稍一思考，哦～～背景颜色太深了，I know！设置个透明度！

![step2](https://dqtwdd.top/cdn/img/20210903160207.png)

搞定收工！

等等！怎么按钮都点不了了？![not easy](http://ww4.sinaimg.cn/bmiddle/005XSXmNgw1farkd1wk3hj305i05iaac.jpg)

遮罩会挡住操作！这时就要出现一个非常重要的 css 属性：`pointer-events`

[pointer-events MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/pointer-events)

将水印层的 dom 设置为`pointer-events: none;`就行。这个问题就算是解决了。

此外，我们肯定不能将水印层设置成纯色，而是需要根据传入的图片和文字来配置。

图片还好说，直接配置`background-img`属性将水印图片设置为背景，然后设置`backgroundRepeat`为 repeat。

至于文字，我们可以使用`canvas`，将文字写在`canvas`之后将带有文字的画布作为图片输入，然后当作背景图片设置为水印层的背景图。

附上完整代码：

```javascript
import './watermark.css';

export function waterMark() {

  const waterMarkDom = document.createElement('div');
  waterMarkDom.className = 'watermark';
  waterMarkDom.setAttribute('id', 'waterMark');

  let canvas = document.createElement('canvas');
  let ctx = canvas.getContext('2d');
  ctx.rotate((20 * Math.PI) / 180);
  ctx.font = '30px Arial';
  ctx.fillText('Hello World', 10, 30);
  let tempSrc = canvas.toDataURL('image/png');
  let img = document.createElement('img');
  img.src = tempSrc;

  waterMarkDom.style.backgroundImage = `URL(${tempSrc})`; //设置背景图的的地址
  waterMarkDom.style.backgroundRepeat = 'repeat'; //设置背景不平铺
  // waterMarkDom.style.backgroundPosition = 'center'; //设置背景图的位置
  waterMarkDom.style.pointerEvents = 'none';
  // waterMarkDom.appendChild(img);
  waterMarkDom.style.position = 'fixed';
  waterMarkDom.style.top = '0';
  waterMarkDom.style.left = '0';
  waterMarkDom.style.right = '0';
  waterMarkDom.style.bottom = '0';
  waterMarkDom.style.opacity = '0.5';

  const shadow = document.createElement('div');
  let shadowDom = shadow.attachShadow({
    mode: 'open',
  });
  shadowDom.append(waterMarkDom);

	document.body.appendChild(shadow);
}

function drawBackgroundTxt() {
  var c = document.getElementById('myCanvas');
  var ctx = c.getContext('2d');
  ctx.font = '30px Arial';
  ctx.fillText('Hello World', 10, 50);
}

// waterMark.css
.watermark {
  position: fixed;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  opacity: 0.5;
}

```

基本的功能就已经开发完毕了，然后继续思考一下有哪些优化点：

1. 水印文字的配置，包括但不限于字体/字号/文字倾斜度/颜色/透明度/粗细等等。

2. 配置水印挂载的 dom。

3. 是否将水印加密，加密方法。

4. 文字在画布中的偏移量。

5. 使用`canvas`的转换的图片是 base64，当使用 js 设置水印的背景属性之后，dom 会显示的非常臃肿，很不美观：

   ![dom](https://dqtwdd.top/cdn/img/20210903170616.png)

   可以配置使用`shadowDom`来改善这个情况：

   [shadowDom](https://developer.mozilla.org/zh-CN/docs/Web/Web_Components/Using_shadow_DOM)

   使用`shadowDom`后：![shadowDom](https://dqtwdd.top/cdn/img/20210903171332.png)

   这样看起来就美观很多了。

6. 前端做水印一个最大的隐患就是安全性的问题，懂一点前端的人都知道可以打开控制台，通过修改 css 隐藏水印，这时我们可以通过使用`Mutation Observer API`监听 dom 的变化，在 dom 变化时让页面报错或者阻止页面重绘，比如：

   ![mutation](https://dqtwdd.top/cdn/img/mutation.gif)

   但是这个做法也不是完全安全的，可以通过禁止 js 执行或者隐藏元素等方法绕过。

总之，前端加水印还是不太安全的，推荐还是后台来做，这个做法只能防止不懂技术的人。
