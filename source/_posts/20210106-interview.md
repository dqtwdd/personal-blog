---
title: 面试准备
tags: ["工作"]
categories: 工作
date: 2021-01-06
---

记录工作中想到的面试应该会的问题。

<!--more-->

## 1. HTML + CSS

### 1.1 HTTP状态码及其含义

* 1XX：信息状态码
  * 100 Continue 继续，一般在发送post请求时，已发送了http header之后服务端将返回此信息，表示确认，之后发送具体参数信息
* 2XX：成功状态码
  * 200 OK 正常返回信息
  * 201 Created 请求成功并且服务器创建了新的资源
  * 202 Accepted 服务器已接受请求，但尚未处理
* 3XX：重定向
  * 301 Moved Permanently 请求的网页已永久移动到新位置。
  * 302 Found 临时性重定向。
  * 303 See Other 临时性重定向，且总是使用 GET 请求新的 URI。
  * 304 Not Modified 自从上次请求后，请求的网页未修改过。
* 4XX：客户端错误
  * 400 Bad Request 服务器无法理解请求的格式，客户端不应当尝试再次使用相同的内容发起请求。
  * 401 Unauthorized 请求未授权。
  * 403 Forbidden 禁止访问。
  * 404 Not Found 找不到如何与 URI 相匹配的资源。
* 5XX: 服务器错误
  * 500 Internal Server Error 最常见的服务器端错误。
  * 503 Service Unavailable 服务器端暂时无法处理请求（可能是过载或维护）。

### 1.2 如何进行网站性能优化

1. 页面懒加载。
2. 使用webpack插件（terser-webpack-plugin），在打包时删除冗余代码 。
3. 使用**SplitChunks** 手动将一些复用性高的文件抽离出来。
4. 使用CDN。
5. 将较大的图片进行无损压缩。

### 1.3 H5新增的特性(只列出了常用的几个)

1. 新增语义化标签 ` header nav footer section ` 

     ``` javaScript
   <header> :头部标签
   <nav> : 导航标签
   <footer> :尾部标签
   <section> :块级标签 
   ```

2. 新增多媒体标签 `audio video  ` 

   ``` javascript
   <audio src="MP3.mp3" controls="controls">您的浏览器不支持Audio元素</audio>
   
   video元素要设定好长宽和src属性就可以了：
   
   <video width="750" height="400" src="time.mp4"></video>
   ```

3. 新增绘图功能 ` canvas   svg` 

### 1.4 `  <!DOCTYPE HTML> `的作用

声明文档类型为HTML
### 1.5  cookies，sessionStorage 和 localStorage 的区别

- cookie是网站为了标示用户身份而储存在用户本地终端（Client Side）上的数据（通常经过加密），一般用来携带
- cookie数据始终在同源的http请求中携带（即使不需要），记会在浏览器和服务器间来回传递
- sessionStorage和localStorage不会自动把数据发给服务器，仅在本地保存
- 存储大小：
  - cookie数据大小不能超过4k
  - sessionStorage和localStorage虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大
- 有效时间：
  - localStorage 存储持久数据，浏览器关闭后数据不丢失除非主动删除数据
  - sessionStorage 数据在当前浏览器窗口关闭后自动删除
  - cookie 设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭

### 1.6 iframe

iframes 提供了一个简单的方式把一个网站的内容嵌入到另一个网站中。

缺点：

* iframe会阻塞主页面的Onload事件
* 搜索引擎的检索程序无法解读这种页面，不利于SEO

### 1.7 行内元素有哪些？块级元素有哪些？ 空(void)元素有那些？行内元素和块级元素有什么区别？

- 行内元素有：a b span img input select strong
- 块级元素有：div ul ol li dl dt dd h1 h2 h3 h4…p
- 空元素：` <br>  `
- 行内元素不可以设置宽高，不独占一行
- 块级元素可以设置宽高，独占一行

### 1.8 display: none;与visibility: hidden;的区别

- 联系：它们都能让元素不可见
- Vue 的`v-show`相当于`display: none` 而 `v-if`是不在dom中渲染该节点
- 区别：

- - display:none;会让元素完全从渲染树中消失，渲染的时候不占据任何空间；visibility: hidden;不会让元素从渲染树消失，渲染师元素继续占据空间，只是内容不可见
  - display: none;是非继承属性，子孙节点消失由于元素从渲染树消失造成，通过修改子孙节点属性无法显示；visibility: hidden;是继承属性，子孙节点消失由于继承了hidden，通过设置visibility: visible;可以让子孙节点显式
  - 修改常规流中元素的display通常会造成文档重排。修改visibility属性只会造成本元素的重绘。
  - 读屏器不会读取display: none;元素内容；会读取visibility: hidden;元素内容

### 1.9 清除浮动

清除浮动的几种方式，各自的优缺点

- 使用空标签清除浮动clear:both（缺点，增加无意义的标签）

- 使用overflow:auto（使用zoom:1用于兼容IE，缺点：内部宽高超过父级div时，会出现滚动条）

- 用afert伪元素清除浮动(IE8以上和非IE浏览器才支持。

  ```html
  // dom
  <div class="main">
   <div class="box blue"></div>
  </div>
  .main {
  border:20px solid blueviolet;
  }
  .main::after {
    content:"";
    display:table; /*采用此方法可以有效避免浏览器兼容问题*/
    clear:both;
  }
  .box {
    width: 100px;
    height: 100px;
  }
  .blue{
    background-color: blue;
    float: left;
  }
  //css
  ```


### 1.10 css3有哪些新特性

- 新增各种css选择器

  ```javascript
  // 伪元素选择器 
  ::before ::after ::first-line ::first-letter ::section  
  // 伪类选择器
  .list>li:first-child     选中第一个子元素
  .list>li:last-child     选中最后一个子元素
  .list>li:nth-child(n)  选中第五个子元素
  .list>li:nth-child(odd)  选中偶数行
  .list>li:nth-child(even)  选中奇数行
  .list>li:nth-child(2n)  支持数学表达式，选中偶数行
  .list>:nth-of-type(1)  选择类型匹配的，选中的是第五个li,跳过ul中其他类型
  ```

  **ps一个坑：伪类选择器，比如`last-child `或者`last-of-type` 的意思是，既要是最后一个，还得是指定的类型**

  [示例链接](https://blog.csdn.net/Dorothy1224/article/details/95206594)

- flex布局

- 圆角 `border-radius`

- 阴影和反射`shadow`

- 文字特效`text-shadow`

- 线性渐变`linear-gradient(#fff2d9, #fff2d9 50%, #f9e7b9 51%, #f9e7b9)`

- 旋转`transform` 包含` scale (缩放)  rotate (旋转)  skew (倾斜) `

- 媒体查询`@media only screen and (min-width: 1200px) {}`

- 过度` transition `

- 动画` animation `

### 1.11 display有哪些值？说明他们的作用

- block 象块类型元素一样显示。
- inline-block 象行内元素一样显示，但其内容象块类型元素一样显示。
- list-item 象块类型元素一样显示，并添加样式列表标记。
- table 此元素会作为块级表格来显示
- inherit 规定应该从父元素继承 display 属性的值

### 1.12 介绍一下标准的CSS的盒子模型？低版本IE的盒子模型有什么不同的？

有两种， IE盒子模型、W3C盒子模型；内容(content)、填充(padding)、边框(border)、边界(margin) ；

- 盒模型(box-sizing:content-box): width/height为content的宽高。
- 怪异模型(box-sizing:border-box)：width/height为content + padding + border的宽高。

### 1.13 position的值， relative和absolute定位原点是

- absolute：生成绝对定位的元素，相对于 static 定位以外的第一个父元素进行定位。
- fixed：生成绝对定位的元素，相对于浏览器窗口进行定位。
- relative：生成相对定位的元素，相对于其正常位置进行定位。
- static 默认值。没有定位，元素出现在正常的流中。
- inherit 规定从父元素继承 position 属性的值。

### 1.14 display:inline-block 出现缝隙解决方案

- 使用margin负值（大概-5px）
- 使用font-size:0

### 1.15 从输入网址，到页面展示，浏览器做了什么

1. DNS解析ip地址。
2. 三次握手建立Tcp链接。
3. 发送Http请求。
4. 服务器处理请求。
5. 服务器返回响应结果。
6. 关闭Tcp链接。
7. 浏览器解析HTML。
8. 浏览器布局渲染页面。

[细说浏览器输入URL后发生了什么](https://segmentfault.com/a/1190000012092552)

### 1.16 请求头的作用

请求头主要用来携带客户端向服务器发送请求的一些描述，比如http协议类型，客户端所接受的编码方式，发送内容的长度等；某些用户信息也携带在请求头中，比如说cookie，token等；还携带了referer信息。

referer信息代表当前请求的源头，可以用来防止盗链，防止恶意请求。

响应头主要用来携带服务器返回的content的一些描述，比如编码类型，内容长度等。

### 1.17 同源策略

浏览器中的**协议、域名、端口号**相同则属于同源。

同源策略是浏览器的一个安全功能，不同源的客户端脚本在没有明确授权的情况下，不能读写对方资源。 

什么要有同源策略：主要是为了保护用户的网络安全。禁止对不同源的DOM进行访问，禁止向不同源的服务器发起HTTP请求。

### 1.18 几种常用css布局

#### 1.18.1 居中

* 文字居中

  * 垂直居中

    1. 为文字添加相等的padding-top和padding-bottom
    2. 单行文字且不会换行展示使`line-height`与容器`height`相等

  * 水平居中

    设置`text-aligin:center`

* `dom`居中

  1. 绝对定位配合`transform`居中

     ```css
     .center {
         left:50%;
         right:50%;
         transform:translate(-50%,-50%)
     }
     ```

  2. 使用`flex`布局

     ```css
     .center {
         display:flex;
         justify-content:center;
         align-items:center;
     }
     ```

#### 1.18.2 两栏布局

1. BFC实现两栏布局

   ```html
   <!--html-->
   <div class="container">
       <div class="left">左边左边左边左边左边左边左边左边左边左边</div>
       <div class="right">右边右边右边右边右边右边右边右边右边右边右边右边右边右边右边右边右边</div>
   </div>
   <!--css-->
   .container {
   	width:100%;
   }
   .left {
   	<!--!important-->
   	float:left;
   	<!--!important-->
   	width:100px;
       background-color: red;
   }
   .right {
   	<!--!important-->
   	overflow:hidden;
   	<!--!important-->
       background-color: blue;
   	min-height:100vh;
   }
   ```

2. flex实现两栏布局

   ```html
   <!--html-->
   <div class="container">
       <div class="left">左边左边左边左边左边左边左边左边左边左边</div>
       <div class="right">右边右边右边右边右边右边右边右边右边右边右边右边右边右边右边右边右边</div>
   </div>
   <!--css-->
   .container {
   	width:100%;
   	<!--!important-->
   	display:flex;
   	<!--!important-->
   }
   .left {
   	width:100px;
   	background-color: red;
   }
   .right {
   	<!--!important-->
   	flex:1;
   	<!--!important-->
   	background-color: blue;
   }
   ```

   

### 1.19 BFC（Block-Formatting-Context块格式化上下文）

* 作用：

  1. 解决外边距合并问题。

     ```html
     <!-- HTML -->
     <div class="container">
         <div class="box red"></div>
         <div class="box blue"></div>
     </div>
     <!-- CSS -->
     .container {
     	position: absolute;
     	border: 5px solid lightcoral;
     }
     .box {
     	width:100px;
     	height:100px;
     	padding:100px;
     }
     .red {
     	background-color:red;
     }
     .blue {
     	background-color:blue;
     }
     ```

     ![OuterMargin](https://gitee.com/dqtwdd/img/raw/master/OuterMargin.jpg)

     以上代码实际展示出来是这个样子的，可以很明显的看出来了两个块之间的间距只有100px，使用BFC的解决办法就是在两个块外层加一个BFC的容器。

     ```html
     <!-- HTML -->
     <div class="container">
         <div class="bfc">
             <div class="box red"></div>
         </div>
         <div class="bfc">
             <div class="box blue"></div>
         </div>
     </div>
     <!-- CSS -->
     .container {
     	position: absolute;
     	border: 5px solid lightcoral;
     }
     .bfc {
     	overflow:hidden
     }
     .box {
      	width:100px;
     	height:100px;
     }
     .red {
     	background-color:red;
     }
     .blue {
     	background-color:blue;
     }
     ```

     ![OuterMarginAfterBFC](https://gitee.com/dqtwdd/img/raw/master/OuterMarginAfterBFC.jpg)

  2. 解决float导致的高度塌陷问题，清除浮动。

     ```html
     <!-- HTML -->
     <div class="container">
     	<div class="box red"></div>
     </div>
     <!-- CSS -->
     .container {
     	border: 5px solid lightcoral;
      	width: 100%;
     }
     .box {
     	width: 100px;
     	height: 100px;
     }
     .red {
     	background-color: red;
     	float: left;
     }
     ```

     ![Float](https://gitee.com/dqtwdd/img/raw/master/Float.jpg)

     ```html
     <!-- HTML -->
     <div class="container">
     	<div class="box red"></div>
     </div>
     <!-- CSS -->
     .container {
     	overflow: hidden;
     	border: 5px solid lightcoral;
     	width: 100%;
     }
     .box {
     	width: 100px;
     	height: 100px;
     }
     .red {
     	background-color: red;
     	float: left;
     }
     ```

     ![FloatAfterBFC](https://gitee.com/dqtwdd/img/raw/master/FloatAfterBFC.jpg)

  3. 用来布局（比如经典的两栏布局）

     ```html
     <!-- HTML -->
     <div class="container">
     	<div class="left"></div>
         <div class="right"></div>
     </div>
     <!-- CSS -->
     .container {
     	width: 100%;
     }
     .left {
     	float:left;
     	min-height:100vh;
     	background-color: red;
     }
     .right {
     	background-color: blue;
     	min-height:100vh;
     	overflow: hidden;
     }
     ```

     ![TwoLayout](https://gitee.com/dqtwdd/img/raw/master/TwoLayout.jpg)

* 形成BFC的条件

  1. 浮动元素，float 除 none 以外的值；
  2. 绝对定位元素，position（absolute，fixed）；
  3. display 为以下其中之一的值 inline-blocks，table-cells，table-captions；
  4. overflow 除了 visible 以外的值（hidden，auto，scroll）

### 1.20 实现一个三角形css

```css
.triangle {
    height:0;
    width:0;
    border:50px solid transparent;
    border-top-color:
}
```

### 1.21 tcp/udp

* tcp 全称 传输控制协议。是一种面向连接的，可靠的，基于字节流的传输通信协议。

  tcp链接过程需要经过三次握手：

  1. 我要发请求了，ok吗？（客户端向服务端发送请求报文段，客户端进入SYN_SEND（请求链接）状态）
  2. ok！我准备好了，发吧！（服务端收到连接请求报文段后，如果同意链接，则发送一个应答，同时客户端进入SYN_RECEIVE（准备接收）状态）
  3. ok，我也准备好了，这就发。（客户端收到链接同意的应答后，还要向服务器发送一个确认报文，发送完成后，客户端进入ESTABLISHED（已连接）状态，服务端接收到请求后也进入ESTABLISHED状态，此时连接建立成功）

  然后就开始了。第三次握手是为了防止出现失效的链接请求报文被服务端接受的情况。

  断开链接过程需要经过四次握手：

  1. 我没啥要发的了，可以断开链接吗？（客户端进入FIN_WAIT_1状态）
  2. 我看看啊，要是没有了我告诉你。（此时，服务端进入CLOSE_WAIT状态，服务端已经不再接受客户端发送的数据，但是仍然可以发送数据给客户端，客户端手收到后进入FIN_WAIT_2状态）
  3. （如果有没发完的继续发送，发送完成后）我没啥发的了，你也断开吧。（此时，服务端进入LAST_ACK状态，等客户端发送ACK确认连接后就关闭链接）
  4. 好的，我也断开了。（此时客户端进入TIME_WAIT状态，在一段时间内没收到B的重发请求的话，就进入CLOSED状态，此条信息发送的就是ACK链接，B收到确认应答后，就进入CLOSED状态）

  TCP特点：

  * 面向连接 是指发送数据之前必须在两端建立链接。
  * 仅支持单播传输 每条TCP传输只能有两个端点，只能进行点对点的数据传输，不支持多播和广播的传输方式。
  * 面向字节流 TCP不像UDP一样以一个一个报文独立的传输，而是在不保留报文边界的情况下以字节流方式进行传输。
  * 可靠传输 TCP为了保证报文传输的可靠，给每一个包一个需要，同时序号也保证了传输到接受端实体包的按序接受，然后接收端对已经接收到的字节发挥一个相应的确认（ACK），如果发送端实体在合理往返延时内未收到确认，那么就会将对应的数据进行重新传送。

* UDP 全称用户数据报协议，是一种无连接协议。UDP具有不提供数据包分组、组装和不能对数据包进行排序的缺点，也就是说，报文发送后，是无法得知是否完整到达的。

应用场景：文件传输等。

UDP特点：

* 面向无连接。 不需要进行连接，想发数据就开始发送。
* 有单播，多播，广播的功能 UDP不只支持一对一的传输方式，还支持一对多，多对多，多对一的方式。
* UDP是面向报文的。
* 不可靠 不知道是否接受，是否有丢包。

应用场景：电话会议，视频等。

|              | UDP                                        | TCP                                    |
| :----------- | :----------------------------------------- | -------------------------------------- |
| 是否连接     | 无连接                                     | 面向连接                               |
| 是否可靠     | 不可靠传输，不使用流量控制和拥塞控制       | 可靠传输，使用流量控制和拥塞控制       |
| 连接对象个数 | 支持一对一，一对多，多对一和多对多交互通信 | 只能是一对一通信                       |
| 传输方式     | 面向报文                                   | 面向字节流                             |
| 首部开销     | 首部开销小，仅8字节                        | 首部最小20字节，最大60字节             |
| 适用场景     | 适用于实时应用（IP电话、视频会议、直播等） | 适用于要求可靠传输的应用，例如文件传输 |

### 1.22 html加载顺序

* 下载的顺序就是从上到下，渲染的顺序也是从上到下，下载和渲染时同时进行的。
* 在渲染到页面的某一部分时，其上面所有部分都已经下载完成（并不是说所有相关联的元素都已经下载完）
* 如果遇到语义解释性的标签嵌入文件（Js脚本，Css样式），那么此时浏览器的下载过程会启用单独的链接进行下载。
* 样式表在下载完成后，将和以前下载的所有样式表一起进行解析，解析完成后，将对此前所有元素（含以前已经渲染的）重新进行渲染。
* Js、Css中如果有重定义，后定义的函数将覆盖前定义的函数。

html：贯穿整个页面；

css（三种声明方式）：

1. 外联样式表：在head标签中，使用link标签的href属性来引用后缀名为.css的css样式文件。
2. 内联样式表：在head标签下的style标签，选择器+样式声明。
3. 外部样式表：在标签的style属性中添加css样式声明。

JavaScript：在`<script>`标签中，可以在head标签汇总，也可以在body标签中。

运行顺序：从上到下运行。

先解析head标签中的代码：

head标签会包含一些引用外部文件的代码，从开始运行就会下载这些被引用的文件。

当遇到`<script>`标签时，浏览器会暂停解析（下载不会暂停），将控制权交给JavaScript引擎（解释器）。

如果`<script>`标签引用了外部脚本，就下载该脚本，否则就直接执行，执行完成后将控制权交给浏览器渲染引擎。

当head中代码解析完毕，会开始解析body中的代码：

如果head中引用的外部文件没有下载完，将会继续下载。

此时如果遇到body标签中的`<script>`，同样会将控制权交给JavaScript引擎来解析JavaScript。

解析完毕后将控制权交还给浏览器渲染引擎

当body中的代码全部执行完毕，并且整个页面的css样式加载完毕后，css会重新渲染整个页面的html。



## 2.JS基础和ES6相关知识

### 2.1 闭包

*  闭包是指有权访问另外一个函数作用域中的变量的函数 
* 闭包的特性：
  * 函数内再嵌套函数
  * 内部函数可以引用外层的参数和变量
  * 参数和变量不会被垃圾回收机制回收
* 说说你对闭包的理解
  * 使用闭包主要是为了设计私有的方法和变量。
  * 闭包的优点是可以避免全局变量的污染。
  * 缺点是闭包会常驻内存，会增大内存使用量，使用不当很容易造成内存泄露。

* 闭包的使用场景
  * 防抖、节流函数
    ```javascript
    // 防抖
    export function debounce (fn, wait, immediate) {
      let timeout
      const immediates = immediate || false
      return function () {
        const context = this
        const args = arguments
        const later = function () {
          timeout = null
          if (!immediates) fn.apply(context, args)
        }
        const call = immediates && !timeout
        clearTimeout(timeout)
        if (call) return fn.apply(context, args)
        timeout = setTimeout(later, wait)
      }
    }
    // 使用 
    function a(b,c) {
      console.log(b,c)
      return b + c
    }
    let de = debounce(a,3,true)
    console.log(de(1,2))
    
    // 节流
    export function throttle (fn, wait) {
      let timeout
      return function () {
        const context = this
        const args = arguments
        if (timeout) return
        timeout = setTimeout(function () {
          fn.apply(context, args)
          timeout = null
        }, wait)
      }
    }
    ```


### 2.2 内存泄漏

* 概念

  当一个对象已经不需要再使用本该被回收时，另外一个正在使用的对象持有它的引用从而导致它不能被回收，这导致本该被回收的对象不能被回收而停留在堆内存中，这就产生了内存泄漏。 

* 造成内存泄露的原因：

  * 意外的全局变量(在函数内部没有使用var进行声明的变量) 
  * console.log 
  * 闭包 
  * 对象的循环引用 
  * 未清除的计时器 
  * DOM泄露(获取到DOM节点之后，将DOM节点删除，但是没有手动释放变量，拿对应的DOM节点在变量中还可以访问到，就会造成泄露)

* 例子

  ```javascript
  // DOM泄露
  //这段代码会导致内存泄露
      window.onload = function(){
          var el = document.getElementById("id");
          el.onclick = function(){
              alert(el.id);
          }
      }
  
  //解决方法为
      window.onload = function(){
          var el = document.getElementById("id");
          var id = el.id; //解除循环引用
          el.onclick = function(){
              alert(id); 
          }
          el = null; // 将闭包引用的外部函数中活动对象清除
      }
  // 闭包导致的内存泄漏
   var fn  =function(){
              var sum = 0
              return function(){
                  sum++
                  console.log(sum);
              }
          }
          fn1=fn() 
          fn1()   //1
          fn1()   //2
          fn1()   //3
  // 这之前，如果fn1一直存在，闭包中的sum就一直不会被内存回收，就造成了内存泄露
          fn1 = null // fn1的引用fn被手动释放了 这样就被回收了
          fn1=fn()  //num再次归零
          fn1() //1
  ```

### 2.3 事件循环机制-----宏任务（macrotask ）和微任务（microtask ）

**下面的重点，只在node版本11之前有体现！！！！！**

**在 node 11 版本中，node 下 Event Loop 已经与浏览器趋于相同。
在 node 11 版本中，node 下 Event Loop 已经与浏览器趋于相同。
在 node 11 版本中，node 下 Event Loop 已经与浏览器趋于相同。 **

**重点来啦：**node环境和浏览器环境对于微任务和宏任务的执行顺序是不一样的，主要体现在：

* node环境中，微任务是事件循环的各个阶段之间执行。
* 浏览器环境中，微任务在事件循环的宏任务执行完成后执行。

![EventLoop](https://gitee.com/dqtwdd/img/raw/master/EventLoop.png)

来一个挺经典的例子：

```javascript
setTimeout(()=>{
    console.log('timer1')
    Promise.resolve().then(function() {
        console.log('promise1')
    })
}, 0)
setTimeout(()=>{
    console.log('timer2')
    Promise.resolve().then(function() {
        console.log('promise2')
    })
}, 0)
// 浏览器 timer1 promise1 timer2 promise2
// node  timer1 timer2 promise1 promise2
```

可以看到以上代码在浏览器中和在node环境中的执行结果是不同的，具体用两张动图说明下原因：

![Browser](https://gitee.com/dqtwdd/img/raw/master/Browser.gif)

![Node](https://gitee.com/dqtwdd/img/raw/master/Node.gif)

* macrotask 和 microtask 表示异步任务的两种分类。

  在挂起任务时，JS 引擎会将所有任务按照类别分到这两个队列中，首先在 macrotask 的队列（这个队列也被叫做 task queue）中取出第一个任务，执行完毕后取出 microtask 队列中的所有任务顺序执行；之后再取 macrotask 任务，周而复始，直至两个队列的任务都取完。

* 宏任务

  | #                     | 浏览器 | Node |
  | --------------------- | ------ | ---- |
  | setTimeout            | √      | √    |
  | setInterval           | √      | √    |
  | setImmediate          | x      | √    |
  | requestAnimationFrame | √      | x    |

* 微任务

  | #                          | 浏览器 | Node |
  | -------------------------- | ------ | ---- |
  | process.nextTick           | x      | √    |
  | MutationObserver           | √      | x    |
  | Promise.then catch finally | √      | √    |

```javascript
//主线程直接执行
console.log('1');
//丢到宏事件队列中
setTimeout(function() {
    console.log('2');
    process.nextTick(function() {
        console.log('3');
    })
    new Promise(function(resolve) {
        console.log('4');
        resolve();
    }).then(function() {
        console.log('5')
    })
})
//微事件1
process.nextTick(function() {
    console.log('6');
})
//主线程直接执行
new Promise(function(resolve) {
    console.log('7');
    resolve();
}).then(function() {
    //微事件2
    console.log('8')
})
//丢到宏事件队列中
setTimeout(function() {
    console.log('9');
    process.nextTick(function() {
        console.log('10');
    })
    new Promise(function(resolve) {
        console.log('11');
        resolve();
    }).then(function() {
        console.log('12')
    })
})
// 打印结果：1 7 6 8 2 4 3 5 9 11 10 12
```

### 2.4 call、apply和bind

* 相同点
  
* 都改变了this的指向
  
* 区别

  * call/apply返回的是函数执行的结果，bind返回的是函数。

    ```javascript
    function personInit (name,age) {
        let person = {
                name:'',
                age:'',
            	getInfo:function (nameParam,ageParam) {
                    console.log(this.name,' ',this.age,' ',nameParam,' ',ageParam)
                }
            }
        person.name = name
        person.age = age
        console.log(this,arguments)
    	return person
    }
    let father = new personInit ('小王',28) 
    let daughter = new personInit ('小小王',2) 
    
    daughter.getInfo() // '小小王' 2
    daughter.getInfo.apply(father) // '小王' 28
    daughter.getInfo.bind(father)() // '小王' 28
    ```

  * call、bind传递第二个参数时可以直接传入，用','隔开，apply传入第二个参数时必须传入[]

    ```javascript
    daughter.getInfo.call(father,'小董',27)  // 小王   28   小董   27
    daughter.getInfo.apply(father,['小董',27])  // 小王   28   小董   27
    daughter.getInfo.bind(father,'小董',27)()  // 小王   28   小董   27
    daughter.getInfo.bind(father,['小董',27])()  // 小王   28   ["小董", 27]  undefined
    ```

### 2.5 原型与原型链

提一个方法：instanceof 

语法：`object instanceof constructor`

作用： `instanceof` 运算符用来检测 `constructor.prototype `是否存在于参数 `object` 的原型链上。 

* 原型

  * 定义

     **每一个javascript对象(除null外)创建的时候，就会与之关联另一个对象，这个对象就是我们所说的原型，每一个对象都会从原型中“继承”属性。**

    **原型是一个** ***对象***。

    * Function => prototype 

      Js中每个函数都有一个prototype属性，这个属性指向函数的**原型**  ***对象***。 

    ```javascript
    function personInit () {
    }
    personInit.prototype.name = '小王'
    let person = new personInit() 
    person.name // '小王'
    ```

    

    * Object => \_\_proto\_\_

      是每个对象(除null外)都会有的属性，叫做\__proto__，这个属性会指向该对象的**原型** ***对象***。 

    ```javascript
    function personInit() {
    }
    let person = new personInit();
    console.log(person.__proto__ === Person.prototype); // true
    ```

    ![Object](https://gitee.com/dqtwdd/img/raw/master/Object.png)

    ​		Object 的 \_\_proto\_\_和 Function 的 prototype 都指向原型对象。

    * constructor

      每个原型都有一个constructor属性，指向该关联的构造函数。 

      ```javascript
      function Person() {
      }
      
      var person = new Person();
      
      console.log(person.__proto__ == Person.prototype) // true
      console.log(Person.prototype.constructor == Person) // true
      console.log(person.constructor === Person); // true
      // 顺便学习一个ES5的方法,可以获得对象的原型
      console.log(Object.getPrototypeOf(person) === Person.prototype) // true
      ```

      ![Constructor](https://gitee.com/dqtwdd/img/raw/master/Constructor.png)

    ![ProtoOfProto](https://gitee.com/dqtwdd/img/raw/master/ProtoOfProto.png)

    Object 的 \_\_proto\_\_ 和 Function 的 prototype 都指向原型对象，修改原型对象参数时，原型对象的构造函数构造出来的对象也会改变。

    当读取实例的属性时，如果找不到，就会查找与对象关联的原型中的属性，如果还查不到，就去找原型的原型，一直找到最顶层为止。

    ```javascript
    function Person() {
    }
    
    var person = new Person();
    
    Person.prototype.__proto__.name = '小王'
    
    let a = {}
    
    console.log(a,a.name) // 小王
    ```

* 原型链

  * 概念

    每个函数都有一个prototype属性，每个函数构造出来的实例都有一个\_\_proto\_\_ 属性，他们都指向构造函数的原型对象，对应的，构造函数的原型对象也有自己的\_\_proto\_\_属性，指向自己的原型，这就是原型链。

    ![ProtoChain](https://gitee.com/dqtwdd/img/raw/master/ProtoChain.png)

    ![proto](https://gitee.com/dqtwdd/img/raw/master/proto.png)

    从上面的图片不难看出，Person和Object这个两个构造函数都是Object.prototype的儿子。

### 2.6 继承

继承就是让子类继承父类的属性。

类是什么，类是对象的抽象（我觉得也是对象）。

继承有六种，分别是 原型链继承，构造继承，实例继承，拷贝继承，组合继承，寄生组合继承。

我们先构造一个父构造函数

```javascript
function Person(name,age) {
    this.name = name || '姓名'
    this.age = age || 999
    this.getName = function () {
        console.log(this.name)
    }
}
// 原型方法
Person.prototype.getAge = function () {
    console.log(this.age)
}
// 实验一下，构造一个实例
let Wdd = new Person('小王',27)
Wdd.getName() // '小王'
Wdd.getAge() // 27
```

#### 2.6.1原型链继承

* 定义：将父类的实例作为子类的原型 。

```javascript
function Black() {
    this.skin = 'black'
    this.hobby = function() {
        console.log('R&B')
    }
}
Black.prototype = new Person()
let black = new Black() // 可以看见，创建子类的实例时，无法向父类的构造函数传参。
black.getName() // '姓名'
black.getAge()  // '999'
console.log(black.skin)  //'black'
black.hobby() // 'R&B'
black instanceof Black  // true
black instanceof Person  // true
```

可以看见Black的实例black继承了所有来自于父类的的属性。

* 特点
  * 简单，容易实现。
  * 原型对象的改变能影响所有后代实例。
* 缺点
  * 无法实现多继承。子类的构造函数只能继承一个父类的属性。
  * 创建子类的实例时，无法向父类的构造函数传参。
  * 来自原型对象的所有属性被所有后代实例共享。

#### 2.6.2 构造继承

* 定义：改变父类构造函数的this指向，使子类获得父类所有的属性和方法。

  区别于拷贝继承的是，构造继承只让子类获取了父类的属性和方法，没有继承父类原型的属性和方法。

  ```javascript
  function Black(name,age) {
      Person.call(this,name,age)
      this.skin = 'black'
      this.hobby = function() {
          console.log('R&B')
      }
  }
  let black = new Black('James',5) 
  black.getName() // 'James'
  black.getAge()  // Uncaught TypeError: black.getAge is not a function
  console.log(black)  //'black'
  black.hobby() // 'R&B'
  black instanceof Black  // true
  black instanceof Person  // false
  ```

* 特点

  * 可以在子类创建时使用父类构造函数传递参数
  * 可以实现多继承
  * 子类的实例就是子类的实例，不是父类的实例。
  * 父类构造函数原型的改变不会影响子类。

* 缺点

  * 父类原型上的的属性和方法不会继承。
  * 无法实现多级集成：比如 祖父类=》父类 使用的是原型链继承，那么父类可以使用祖父的所有属性和方法，子类=》父类使用构造继承的话 子类是无法使用祖父类的方法和属性的。每一个子类如果想要使用祖父类和父类的话需要分别进行。

#### 2.6.3 实例继承（寄生继承）

* 定义：使用父类构造函数创建实例，为实例添加新特性后，作为子类实例返回。

  这个就很神奇，就如同它的名字一样，通过子类构造函数创造的实例却不是子类的实例

  ``` javascript
  function Black() {
      let instance = new Person()
      instance.skin = 'black'
      instance.hobby = function() {
          console.log('R&B')
      }
      return instance
  }
  let black = new Black()
  black.getName() // 'James'
  black.getAge()  // Uncaught TypeError: black.getAge is not a function
  console.log(black)  //'black'
  black.hobby() // 'R&B'
  black instanceof Black  // false
  black instanceof Person  // true
  ```

* 特点

  * 创建的实例时父类的实例，不是子类的实例。

* 缺点

  * 不支持多继承。

#### 2.6.4 拷贝继承

* 定义：创建父类的实例后，遍历父级实例，将父级实例原型上的方法赋给子类构造函数的原型对象。

  ```javascript
  function Black() {
      let instance = new Person()
      for(let key in instance) {
          Black.prototype[key] = instance[key]
      }
      this.skin = 'black'
      this.hobby = function() {
          console.log('R&B')
      }
  }
  let black = new Black()
  black.getName() // '姓名'
  black.getAge()  // '999'
  black.hobby() // 'R&B'
  black instanceof Black  // true
  black instanceof Person  // false
  ```

* 特点

  * 支持多继承。
  * 可以继承父类的原型方法，但是不是父类的实例。

* 缺点

  * 执行效率低（因为需要遍历拷贝父类的每一条属性）。
  * 无法复制不可遍历属性。

#### 2.6.5 组合继承

* 定义：组合继承即组合了原型链继承和构造继承，使子类即可以向父类构造函数传递参数，又可以能继承父类原型的属性和方法。

  ```javascript
  function Black(name,age) {
      Person.call(this,name,age)
      this.skin = 'black'
      this.hobby = function() {
          console.log('R&B')
      }
  }
  Black.prototype = new Person()
  
  Black.prototype.constructor = Black // 修改子类原型构造函数
  // 子类原型的构造函数不影响子类创建实例，但是我们一般会修正子类原型的构造函数使其指向本身。
  
  let black = new Black('Jone',5)
  black.getName() // 'Jone'
  black.getAge()  // '5'
  black.hobby() // 'R&B'
  black instanceof Black  // true
  black instanceof Person  // true
  ```

* 特点

  * 子类即可以向父类构造函数传递参数，又可以能继承父类原型的属性和方法。
  * 既是子类实例，又是父类实例。

* 缺点

  * 调用了两次父类的构造函数，创建了两次父类的实例。

#### 2.6.6 寄生组合继承

* 定义：使用构造继承使子类可以使用父类的属性和方法，通过继承方法创建一个父类构造函数的副本使子类的原型指向这个副本，以此访问父类的原型。

  ```javascript
  function Black(name,age) {
      Person.call(this,name,age)
      this.skin = 'black'
      this.hobby = function() {
          console.log('R&B')
      }
  }
  function inherit(subType,superType) {
      //在new inheritFn 的时候将构造函数指向子类
      function inheritFn() {this.constructor = subType }
      inheritFn.prototype = superType.prototype
       //将子类的原型指向父类原型的一个副本
      subType.prototype = new inheritFn()
  }
  
  inherit(Black,Person)
  
  let black = new Black('Jone',5)
  black.getName() // 'Jone'
  black.getAge()  // '5'
  black.hobby() // 'R&B'
  black instanceof Black  // true
  black instanceof Person  // true
  ```

* 特点

  * 可以实现多继承，但是原型只能指向一个父类的原型。

* 缺点

  * 实现比较麻烦。

### 2.7 深拷贝、浅拷贝

#### 2.7.1 基本数据类型：number string boolean undefined null object

* 简单数据类型：number string boolean undefined null
* 复杂数据类型：object

简单数据类型存放在**栈**中，负责数据类型存放在**堆**中。

复杂数据类型赋值时会指向同一个内存地址（既浅拷贝）：

```javascript
let objAncestor = {
    name:'ancestor',
    age:999
}
let objGeneration = objAncestor
console.log(objGeneration.name) // 'ancestor'
objAncestor.name = 'son'
console.log(objGeneration.name) // 'son'
```

简单数据类型就没有这样的问题：

```javascript
let a = 10
let b = a
a = 11
console.log(a,b) // 11 10
```

#### 2.7.2 typeof与instanceof

* typeof 主要用于判断简单数据类型，只能返回 `'number' 'boolean' 'string' 'null' 'undefined' 'object'`

  ```javascript
  typeof '' // 'string'
  typeof 1 // 'number'
  typeof []  // 'object'
  typeof null  // 'object' 历史遗留问题
  typeof true  // 'boolean'
  typeof undefinded  // 'undefined'
  typeof function() {}  //'function'
  ```

* instanceof 可以判断是否为某个构造函数的实例，但是对简单数据类型的判断不准确

  ```javascript
  [] instanceof Object  //true
  [] instanceof Array  //true
  {} instanceof Object  //true
  /\d/ instanceof RegExp  //true
  function(){} instanceof Object  //true
  function(){} instanceof Function  //true
  
  '' instanceof String  //false
  1 instanceof Number //false
  ```

* 比较好的办法是使用`Object.prototype.toString.apply()`来判断数据类型。

  ```javascript
  Object.prototype.toString.apply([1,2])    //"[object Array]"
  Object.prototype.toString.apply('str')    //"[object String]"
  Object.prototype.toString.apply(1)        //"[object Number]"
  Object.prototype.toString.apply(null)     //"[object Null]"
  Object.prototype.toString.apply()         //"[object Undefined]"
  Object.prototype.toString.apply(function(){})      //"[object Function]"
  Object.prototype.toString.apply(true)     //"[object Boolean]"
  Object.prototype.toString.apply(new Object())     //"[object Object]"
  ```

#### 2.7.3 深拷贝

*  `Object.assign()`方法用于将所有可枚举属性的值从一个或多个源对象分配到目标对象。它将返回目标对象。 此方法可以实现对象内部第一层简单数据类型的深拷贝。

  ```javascript
  let a = {a:1,b:2,c:{d:1}}
  let b = Object.assign({},a) // 参数中 {} 开辟了新的内存空间，将a中的简单数据类型拷过去了
  a.a = 2
  a.c.d = 2
  console.log(a) // {a:2,b:2,c:{d:2}}
  console.log(b) // {a:1,b:2,c:{d:2}} 可以看见第一层还是ok的，第二层不行。
  ```

* `JSON.stringify`和`JSON.parse`实现深拷贝：JSON.stringify把对象转成字符串，再用JSON.parse把字符串转成新的对象；

  这个方法的缺点是只能实现转换成Json格式的才能用这种方式，如果源中含有function的这种方式是拷贝不过去的。

  不过这种方式工作中一般的情况都可以应付了。

  ```javascript
  // 例1：
  let a = {a:1,b:2,c:{d:1}}
  function clone (source) {
      let obj = JSON.parse(JSON.stringify(source))
      return obj
  }
  let b = clone(a) 
  a.a = 2
  a.c.d = 2
  console.log(a) // {a:2,b:2,c:{d:2}}
  console.log(b) // {a:1,b:2，c:{d:1}} 
  // 例2：
  let a = {a:1,b:2,c:function(){console.log('this is a Function')}}
  function clone (source) {
      let obj = JSON.parse(JSON.stringify(source))
      return obj
  }
  let b = clone(a) 
  a.a = 2
  a.c.d = 2
  console.log(a) // {a:1,b:2,c:function(){console.log('this is a Function')}}
  console.log(b) // {a:1,b:2} 可以看见c直接蒸发了
  ```

* 递归实现深拷贝

  ```javascript
  let a = {a:1,b:2,c:function(){
      console.log('this is a Function')
  }}
  function deepClone(source) {
      let newObj
      if(source !== null && typeof source === 'object') {
      	newObj = Object.prototype.toString.call(source) === '[object Array]'? []:{}
          for (let key in source) {
              newObj[key] = deepClone(source[key])
          }
      } else {
          newObj = source
      }
      return newObj
  }
  let b = deepClone(a) 
  a.a = 2
  a.c.d = 2
  console.log(a) // {a:2,b:2,c:{d:2}}
  console.log(b) // {a:1,b:2，c:{d:1}}
  a.c = function () { console.log('this is not a Function') }
  a.c() // 'this is not a Function'
  b.c() // 'this is a Function'
  ```

  

附带一个自己做题时遇到的小问题：[一道简单小题引发的思考](https://dqtwdd.gitee.io/2020/08/12/20200812-mergeArr/)

### 2.8 new 关键字做了什么

1. 创建一个新对象。
2. 将新对象的_proto_指向构造函数的prototype对象。
3. 将构造函数的作用域赋值给新对象 （也就是this指向新对象）。
4. 执行构造函数中的代码（为这个新对象添加属性）。
5. 返回新的对象。

```javascript
let people = new Person()
==========================================
let people = {}
people.__proto__ = Person.prototype
Person.call(people)
return people
```

### 2.9 0.1 + 0.2 为什么不等于0.3

因为Js采用的是**64位双精度浮点数**编码，所以会有精度问题。

```javascript
0.1 + 0.2 // 0.30000000000000004  (15个0)
0.7 * 180 // 125.99999999998
1000000000000000128 === 1000000000000000129 //true  (15个0)
```

解决方案：小数计算时将小数转换成整数进行计算后再恢复成小数。

### 2.10 事件捕获和事件冒泡

事件流程处理分为三个阶段：

1. 事件捕获
2. 事件目标
3. 事件冒泡

```javascript
// addEventListener，他有一个参数，为true就是捕获阶段，为false就是冒泡阶段，默认为false
<div id="box1">
    <div id="box2">
        <div id="box3"></div>
    </div>
</div>
// js 结构
document.getElementById('box1').addEventListener('click', () => {
       console.log('box1事件捕获阶段')
   },true)
document.getElementById('box1').addEventListener('click', () => {
       console.log('box1事件冒泡阶段')
   },false)
document.getElementById('box2').addEventListener('click', () => {
       console.log('box2事件捕获阶段')
   },true)
document.getElementById('box2').addEventListener('click', () => {
       console.log('box2事件冒泡阶段')
   },false)
document.getElementById('box3').addEventListener('click', () => {
       console.log('box3事件捕获阶段')
   },true)
document.getElementById('box3').addEventListener('click', () => {
       console.log('box3事件冒泡阶段')
   },false)
// 控制台打印
box1事件捕获阶段
box2事件捕获阶段
box3事件捕获阶段
box3事件冒泡阶段
box2事件冒泡阶段
box1事件冒泡阶段
```

* 事件捕获：点击一个dom后，事件会从文档的根节点流向目标节点，触发事件捕获（也就是`addEventListener true `时监听到的事件。）
* 事件目标：当事件到达目标节点后，就会触发目标节点所绑定的事件。然后会开始逆向回流，直到文档的最外层。
* 事件冒泡：目标节点绑定的事件触发后，并不会终止，而是会向文档的外层冒泡，依次触发外层dom所绑定的事件。

事件委托就是基于这个原理，当一个列表中有很多项时，我们可以不用为每一项绑定一个`@click`事件，而是可以直接在父节点绑定一个事件，传入`e`，然后对目标节点进行判断后做出回应：

```javascript
//html
<table @click="edit">
  <tr v-for="item in list">
    <td>{{item.name}}</td>
    ...
    <td>
      <button :data-id="item.id" title="eidt">编辑</button>
   </td>
  </tr>
</table>

//js
edit (event){
    if(event.target.title == "edit"){ //如果点击到了edit 
        let id = evenr.target.dataset.id;
        //拿着id参数执行着相关的操作
    }
}
```

**无法在捕获阶段阻止事件冒泡！ **

vue中可以使用修饰符来`@click.prevent` 阻止默认事件，使用` @click.stop`阻止事件冒泡。

### 2.11 ajax原理

**ajax：** 既 *Asynchronous JavaScript and XML* 异步的Javascript 和 XML。

ajax不是一种新技术，而是以下几种技术的组合：

1. 使用**HTML**和**Css**进行展示。
2. 使用**Dom**模型来交互和显示。
3. 使用**XMLHttpRequest**来请求。
4. 使用**JavaScript**来绑定和调用。

通过下图可以看出，以前的页面发送请求时是返回整个页面的信息，使用Ajax之后可以极大的降低网络请求传输的数据大小。

![ajax原理](https://gitee.com/dqtwdd/img/raw/master/AjaxPrinciple.png)

### 2.12 前端模块化

* Node采用CommonJS规范

  * 使用`module.exports`和`require()`配对
  * 特点：
    1. 每一个文件就是一个模块。
    2. 每个模块有自己的作用域。
    3. 每个模块内部的变量，方法，都是私有的，只能通过exports属性访问。

  看着眼熟不眼熟，面向对象啊！感觉每个module就是一个对象。

  ```javascript
  // example.js
  var x = 5;
  var addX = function (value) {
    return value + x;
  };
  module.exports.x = x;
  module.exports.addX = addX;
  
  // main.js
  var example = require('./example.js');
  
  console.log(example.x); // 5
  console.log(example.addX(1)); // 6
  ```

* ES6模块规范

  * 使用`export`和`import`配对。
  * 特点：
    *  `export`命令规定的是对外的接口，必须与模块内部的变量建立一一对应关系。 

  ```javascript
  // 报错
  export 1;
  
  // 报错
  var m = 1;
  export m;
  
  // 写法一
  export var m = 1;
  
  // 写法二
  var m = 1;
  export {m};
  
  // 写法三
  var n = 1;
  export {n as m};
  
  
  // 报错
  function f() {}
  export f;
  
  // 正确
  export function f() {};
  
  // 正确
  function f() {}
  export {f};
  ```

### 2.13 ES6

#### 2.13.1 async await

*  `async`是  Generator  函数的语法糖。
*  `async`可以将异步函数改写为同步形式。
* `await` 后面跟一个函数。
*  `async`函数返回一个 Promise 对象，可以使用`then`方法添加回调函数。 
* 只有在 `async`内部函数执行完成后，函数才会继续往后执行。
* `await`后面如果是一个Promise，会等待返回`resolve`或者`reject`之后再继续执行。
* `await`后面如果是一个普通函数，会立即执行。

```javascript
function fn1 () {
    console.log(1)
}
function fn2 () {
    console.log(2)
    setTimeout(fn1,10)
    console.log(3)
}
function fn3 () {
    return new Promise(function(resolve) {
    	console.log('4');
   		resolve();
	}).then(function() {
    	console.log('5')
	})
}
async function fn4 () {
    let res = await fn3()
    fn1()
    fn2()
}
async function fn5 () {
    let res = await fn2()
    fn1()
    fn2()
}
fn4()  // 4 5 1 2 3 1
fn5()  //2 3 1 2 3 1 1

```

#### 2.13.2 set map

1. set 

   * `set` 类似于数组，与数组不同的是它内部的每个值都是唯一的。
   * `set`本身是一个构造函数，可以通过`new`关键字创建`set`对象。 

   ```javascript
   const s = new Set();
   
   [2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x));
   
   for (let i of s) {
     console.log(i);
   }
   // 2 3 5 4
   
   // 例一
   const set = new Set([1, 2, 3, 4, 4]);
   [...set]
   // [1, 2, 3, 4]
   
   // 例二
   const items = new Set([1, 2, 3, 4, 5, 5, 5, 5]);
   items.size // 5
   ```

   * `set`的属性

     * `Set.prototype.size`用于查看`set`的总成员数，类似`Array.prototype.length`
     * `Set.prototype.constructor`用于查看`set`的构造函数。

   * `set`的方法

     * `Set.prototype.has`用于查看`set`是否含有某个元素。

       ```javascript
       let set = new Set([1,2,3,3,4,5,5,6])
       set.has(1)  //true
       set.has('1')  //false
       ```

     * `Set.prototype.add`用于向`set`添加新元素。

       ```javascript
       console.log([...set])  // [1,2,3,4,5,6]
       set.add(9)
       console.log([...set])  // [1,2,3,4,5,6,9]
       set.add(1)
       console.log([...set])  // [1,2,3,4,5,6,9]
       ```

     * `Set.prototype.delete用于`set`删除元素。

       ```javascript
       console.log([...set])  // [1,2,3,4,5,6,9]
       set.delete(9)
       console.log([...set])  // [1,2,3,4,5,6]
       set.delete(1)
       console.log([...set])  // [2,3,4,5,6]
       ```

     * `Set.prototype.clear`用于清除`set`所有元素。

2. `map`

   * `map`类似于对象，与对象不同的是，传统的对象的`key`只能是String，而`map`的`key`可以是任意类型（`Object Array String ... `）。

   * `map`的属性

     `size`属性返回 Map 结构的成员总数。

   * `map`的方法

     * ` Map.prototype.set(key, value) `： `set`方法设置键名`key`对应的键值为`value`，然后返回整个 Map 结构。如果`key`已经有值，则键值会被更新，否则就新生成该键。 
     * ` Map.prototype.has(key)`： `has`方法返回一个布尔值，表示某个键是否在当前 Map 对象之中。 
     * `Map.prototype.get(key)`：`get`方法读取`key`对应的键值，如果找不到`key`，返回`undefined`。
     * ` Map.prototype.delete(key)` ： `delete`方法删除某个键，返回`true`。如果删除失败，返回`false`。 
     * ` Map.prototype.clear()： `： `clear`方法清除所有成员，没有返回值。 

#### 2.13.3 let和const

先看一道经典的面试题

```javascript
for (var i = 0; i <10; i++) {  
  setTimeout(function() {
    console.log(i);        
  }, 0);
}
// 输出结果
10 10 10 10 10 10 10 10 10 10 
```

以前啊，把这道题想简单了，以为知识简单的考`let`和`var`的区别，昨天跟小伙伴讨论的时候才意识到这道题考到了块级作用域和宏任务。现在把这道题改变一下，然后剖析一下。

```javascript
for (var i = 0; i <10; i++) {  
  console.log('print',i)
  setTimeout(function() {
    console.log(i);        
  }, 0);
}
// 输出结果
print1 print2 print3 print4 print5 print6 print7 print8 print9 10 10 10 10 10 10 10 10 10 10
```

结合后面提到的宏任务，我们可以分析，每一次执行循环的时候，都是先执行了一遍`console.log('print',i)`，然后把`setTimeout`的内容拿出来，放到宏任务的队列里面，由于`var`没有块级作用域，在执行完`for`循环后此时`i`的值为10，然后再执行宏任务，打印10次10。

而将`var`改为`let`之后，由于块级作用域的作用，每一次`for`的`i`不会互相影响， 所以打印结果就变成了`print1 print2 print3 print4 print5 print6 print7 print8 print9 1 2 3 4 5 6 7 8 9`

### 2.14 常用的`array string`方法

* `array`
  1. `Array.splice()` 截取数组中的一段，并返回新数组。
  2. `Array.concat()` 连接连个新数组。
  3. `Array.push()` 向数组最后添加一个元素。
  4. `Array.pop()` 删除并返回数组的最后一个元素。
  5. `Array.reverse()` 将数组倒叙排列。
  6. `Array.forEach()` 对数组中的每一个元素执行一遍函数，无返回值，更改原数组。
  7. `Array.every()` 检查数组中的每个元素是否都符合条件。
  8. `Array.map()` 对数组中的每一个元素执行一遍函数，返回一个新数组，不改变原数组。
  9. `Array.includes()` 查看数组是否包含某个元素。
  10. `Array.findIndex()` 查找数组中某个元素的索引。
  11. `Array.shift()` 从删除并返回一个元素。
  12. `Array.toString()` 将两个数组连接成一个新数组。
  13. `Array.sort()` 将数组排序。 
  14. `Array.split()`从数组中添加或者删除元素。 
* `string`
  1. `String.split()` 将字符串分割为数组。
  2. `String.replace()` 替换指定字符串。
  3. `String.slice()` 截取指定长度字符串并返回。
  4. `String.includes()` 判断字符串是否包含某个指定字符。
  5. `String.concat()` 将两个字符串链接成一个。
  6. `String.toLowerCase()` 转换小写。
  7. `String.toUpperCase()` 转换大写。
  8. `String.indexOf()` 查找指定字符第一次出现的位置。

### 2.15 Promise 的静态方法

1. `Promise.prototype.then()`

   `Promise.prototype.then()`是`Promise`返回`resolve`时的回调。

2. `Promise.prototype.catch()`

   `Promise.prototype.catch()`是`Promise`返回`reject`时的回调。

3. `Promise.prototype.finally()`

   `Promise.prototype.finally()`不关心`Promise`的返回是`resolve`还是`reject`，这个函数在`Promise`有返回值后执行。

4. `Promise.all()`

   `Promise.all()`方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。 

   `Promise.all()`的参数是一个数组，数组成员都是`Promise`实例。返回结果有两种情况：

   1. 参数中所有的`Promise`实例都返回`resolve`，则`Promise.all()`返回`fulfilled`，返货值为一个数组，数组成员为各个`Promise`实例`resolve`的值。
   2. 只要有一个返回`reject`，则直接执行`Promise.all()`的`.catch()`。

   ```javascript
   const p1 = new Promise((resolve, reject) => {
     resolve('hello');
   })
   
   const p2 = new Promise((resolve, reject) => {
     resolve('world');
   })
   
   const p3 = new Promise((resolve, reject) => {
     throw new Error('报错了');
   })
   
   Promise.all([p1, p2])
   .then(result => console.log(result))
   .catch(e => console.log('catch error:',e));
   // ["hello", "world"]
   
   Promise.all([p1, p3])
   .then(result => console.log(result))
   .catch(e => console.log('catch error:',e));
   // catch error: Error: 报错了
   ```

   

   **注意：**如果返回`reject`的`Promise`实例有自己的`.catch()`方法，那么并不会执行`Promise.all()`的`.catch()`方法，而是作为`fulfilled`执行`.then()`

   ```javascript
   const p1 = new Promise((resolve, reject) => {
     resolve('hello');
   })
   .then(result => result)
   .catch(e => e);
   
   const p2 = new Promise((resolve, reject) => {
     throw new Error('报错了');
   })
   .then(result => result)
   .catch(e => e);
   
   Promise.all([p1, p2])
   .then(result => console.log(result))
   .catch(e => console.log(e));
   // ["hello", Error: 报错了]
   ```

5. `Promise.race()`

   `Promise.race()`和`Promise.all()`接受的参数相同，也是一个`Promise`实例数组，不同的是，`Promise.race()`中只要有一个返回`resolve`或者`reject`就直接执行`Promise.race()`的`.then()`或者`.catch()`方法。不等待其他的实例完成。

   ```javascript
   const p1 = new Promise((resolve, reject) => {
     resolve('hello');
   })
   const p2 = new Promise((resolve, reject) => {
     resolve('world');
   })
   const p3 = new Promise((resolve, reject) => {
     throw new Error('报错了');
   })
   Promise.race([p1,p2,p3])
   .then(result => console.log(result))
   .catch(e => console.log('catch error:',e));
   // hello
   ```

6. `Promise.allSettled()`

   `Promise.allSettled()`和`Promise.all()`接受的参数相同，也是一个`Promise`实例数组，不同的是，`Promise.allSettled()`不关心参数中每个`Promise`实例返回的是`resolve`还是`reject`，关心的是数组中所有的`Promise`对象都完成了，该方法的返回值是参数中各个`Promise`实例的返回结果组成的数组。

   ```javascript
   const p1 = new Promise((resolve, reject) => {
     resolve('hello');
   })
   const p2 = new Promise((resolve, reject) => {
     resolve('world');
   })
   const p3 = new Promise((resolve, reject) => {
     throw new Error('报错了');
   })
   Promise.all([p1,p2,p3])
   .then(result => console.log(result))
   .catch(e => console.log('catch error:',e));
   // [{"status":"fulfilled","value":"hello"},{"status":"fulfilled","value":"world"},{"status":"rejected","reason":{}}]
   ```

7. `Promise.any()`

   `Promise.any()`和`Promise.all()`接受的参数相同，也是一个`Promise`实例数组，不同的是，`Promise.any()`中只要有一个`Promise`实例返回`resolve`，`Promise.any()`就返回`resolve`，并执行`.then()`后面的方法，所有`Promise`实例都返回`reject`，则返回`reject`，`Promise.any()`返回的错误是一个` AggregateError  `错误实例数组。

   ```javascript
   const p1 = new Promise((resolve, reject) => {
       throw new Error('报错了');
   })
   const p2 = new Promise((resolve, reject) => {
     throw new Error('报错了');
   })
   const p3 = new Promise((resolve, reject) => {
     throw new Error('报错了');
   })
   Promise.any([p1,p2,p3])
   .then(result => console.log(result))
   .catch(e => console.log('catch error:',e));
   // catch error: AggregateError: All promises were rejected
   
   const p1 = new Promise((resolve, reject) => {
       throw new Error('报错了');
   })
   const p2 = new Promise((resolve, reject) => {
     throw new Error('报错了');
   })
   const p3 = new Promise((resolve, reject) => {
     resolve('hello')
   })
   Promise.any([p1,p2,p3])
   .then(result => console.log(result))
   .catch(e => console.log('catch error:',e));
   // hello
   ```


### 2.16 Js包含哪几部分？

核心（ECMAScript）、文档对象模型（DOM）、浏览器对象模型（BOM）

常见的BOM对象：

window：代表整个浏览器窗口（window是BOM中的一个对象，并且是顶级的对象）

Navigator ：代表浏览器当前的信息，通过Navigator我们可以获取用户当前使用的是什么浏览器

Location： 代表浏览器当前的地址信息，通过Location我们可以获取或者设置当前的地址信息

History：代表浏览器的历史信息，通过History我们可以实现上一步/刷新/下一步操作（出于对用户的隐私考虑，我们只能拿到当前的浏览记录，不能拿到所有的历史记录）

Screen：代表用户的屏幕信息

### 2.17 设计模式：

#### 2.17.1 工厂模式

#### 2.17.2 观察者模式

#### 2.17.3 订阅发布模式

#### 2.17.4 单例模式

#### 2.17.5 装饰器模式

### 2.18 for in 与 for of

for in 遍历的是key，for of 遍历的是value。

### 2.19 执行上下文

JavaScript有三种执行上下文类型：

* 全局执行上下文

  一个程序中只会有一个全局执行上下文，它会执行两件事情：

  1. 创建一个全局的window对象（浏览器情况下；
  2. 设置this的值等于这个全局对象。

* 函数执行上下文

  每当一个函数被调用时，都会为该函数创建一个新的上下文，每个函数都有它自己的执行上下文，不过是在函数调用的时候创建的；函数的上下文可以有任意多个，每当一个新的执行上下文被创建，它会按照定义的顺序执行一系列步骤。 

* Eval函数执行上下文

### 2.20 JS GC（Garbage Collection）垃圾回收机制

* 可达性 

  js中“可达性”值就是那些可以以某种方式访问或可用的值，他们被保证储存在内存中。

* 垃圾是什么

  一般来说，没有被引用到的对象就是垃圾，就是需要被清除的，如果几个对象引用成一个环，互相引用，但根访问不到他们，这几个对象也是垃圾，也要被清除。

* 标记-清除 算法

  1. 标记阶段：从根集合出发，将所有活动对象及其子对象打上标记。
  2. 清除阶段：遍历堆，将非活动对象（未打上标记）清除。

参考：[思否-垃圾回收机制](https://segmentfault.com/a/1190000018605776)

### 2.21 Polyfill

Polyfill字面意思是垫片，意为用于实现浏览器并不支持的原生AP的代码。

常见的Polyfill有比如new函数，debounce防抖函数，throttle节流函数等。

### 2.22 函数柯里化

柯里化是一种函数的转变，它指将一个函数从可调用的f(a,b,c)转换为可调用的f(a)(b)(c)。

柯里化不会调用函数，他只是对函数进行转换。

柯里化的核心思想是：降低通用性，提高适用性。 

例：正则判断

如下，封装一个正则判断的函数，传入正则表达式和目标字符串，返回判断结果。没有问题。

```jsx
function check(targetString, reg) {
    return reg.test(targetString);
}

check(/^1[34578]\d{9}$/, '14900000088');
check(/^(\w)+(\.\w+)*@(\w)+((\.\w+)+)$/, 'test@163.com');
```

当业务需求只是判断手机号或判断邮箱时，仍需要每次都传入相应的正则这就很低效，因此可以使用柯里化函数再次进行封装。

```jsx
function curring(reg) {
  return (str) => {
    return reg.test(str);
  };
}
var checkPhone = curring(/^1[34578]\d{9}$/);
var checkEmail = curring(/^(\w)+(\.\w+)*@(\w)+((\.\w+)+)$/);

console.log(checkPhone("183888888")); // false
console.log(checkPhone("17654239819")); // true
console.log(checkEmail("exy@163.com")); // true
```



## 3.Vue

### 3.1 前端如何解决跨域问题

跨域的几种方式：

1. JSONP跨域。
2. CORS跨域。
3. Websocket跨域。

一般跨域是后端配置的。

前端在测试时可以在`vue.config.js`中配置代理，可以在本地调用接口。

```javascript
// vue.config.js
const isDev = process.env.VUE_APP_ENV === "development";
const path = require("path");

module.exports = {
  publicPath: "./",
  productionSourceMap: isDev,
  pages: {},
  chainWebpack: config => {},
  devServer: {
    proxy: {
      "/": {
        target: "https://sspuat.taikang.com",
        changeOrigin: true
      }
    }
  }
};
```



### 3.2  MVVM

MVVM与MVC的最大区别是：**MVVM实现了View和Model的自动同步**，也就是当Model的属性改变时，我们不用再自己手动操作Dom元素来改变View的显示，而是改变属性后该属性对应View层显示会自动改变。 

MVVM解决了：

1. MVC中大量的DOM操作使页面渲染性能降低，加载速度变慢，影响用户体验的问题。
2. 每次Module更新数据需要用户手动更新View的问题。

### 3.3 vue双向绑定原理

原理：vue是使用**数据劫持**结合**订阅-发布模式**实现双向绑定的。通过`Object.defineProperty()`方法劫持数据对象的`get()`和`set()`方法，可以理解为，在`get()`中执行的就是订阅过程，在`set()`中执行的就是发布过程。\

订阅发布模式和观察者模式

![两种开发模式](https://gitee.com/dqtwdd/img/raw/master/Publish-Observer.png)

* 订阅-发布模式

  ```javascript
      //定义一家猎人工会
  	//主要功能包括任务发布大厅(topics)，以及订阅任务(subscribe)，发布任务(publish)
  	let HunterUnion = {
  		type: 'hunt',
  		topics: Object.create(null),
  		subscribe: function (topic, fn){
  		    if(!this.topics[topic]){
  		      	this.topics[topic] = [];  
  		    }
  		    this.topics[topic].push(fn);
  		},
  		publish: function (topic, money){
  		    if(!this.topics[topic])
  		      	return;
  		    for(let fn of this.topics[topic]){
  		    	fn(money)
  		    }
  		}
  	}
  	
  	//定义一个猎人类
  	//包括姓名，级别
  	function Hunter(name, level){
  		this.name = name
  		this.level = level
  	}
  	//猎人可在猎人工会发布订阅任务
  	Hunter.prototype.subscribe = function (topic, fn){
  		console.log(this.level + '猎人' + this.name + '订阅了狩猎' + topic + '的任务')
  	    HunterUnion.subscribe(topic, fn)
  	}
  	Hunter.prototype.publish = function (topic, money){
  		console.log(this.level + '猎人' + this.name + '发布了狩猎' + topic + '的任务')
  	    HunterUnion.publish(topic, money)
  	}
  	
  	//猎人工会走来了几个猎人
  	let hunterMing = new Hunter('小明', '黄金')
  	let hunterJin = new Hunter('小金', '白银')
  	let hunterZhang = new Hunter('小张', '黄金')
  	let hunterPeter = new Hunter('Peter', '青铜')
  	
  	//小明，小金，小张分别订阅了狩猎tiger的任务
  	hunterMing.subscribe('tiger', function(money){
  		console.log('小明表示：' + (money > 200 ? '' : '不') + '接取任务')
  	})
  	hunterJin.subscribe('tiger', function(money){
  		console.log('小金表示：接取任务')
  	})
  	hunterZhang.subscribe('tiger', function(money){
  		console.log('小张表示：接取任务')
  	})
  	//Peter订阅了狩猎sheep的任务
  	hunterPeter.subscribe('sheep', function(money){
  		console.log('Peter表示：接取任务')
  	})
  	
  	//Peter发布了狩猎tiger的任务
  	hunterPeter.publish('tiger', 198)
  	
  	//猎人们发布(发布者)或订阅(观察者/订阅者)任务都是通过猎人工会(调度中心)关联起来的，他们没有直接的交流。
  ```

* 观察者模式

  ```javascript
      //有一家猎人工会，其中每个猎人都具有发布任务(publish)，订阅任务(subscribe)的功能
  	//他们都有一个订阅列表来记录谁订阅了自己
  	//定义一个猎人类
  	//包括姓名，级别，订阅列表
  	function Hunter(name, level){
  		this.name = name
  		this.level = level
  		this.list = []
  	}
  	Hunter.prototype.publish = function (money){
  		console.log(this.level + '猎人' + this.name + '寻求帮助')
  	    this.list.forEach(function(item, index){
  	    	item(money)
  	    })
  	}
  	Hunter.prototype.subscribe = function (targrt, fn){
  		console.log(this.level + '猎人' + this.name + '订阅了' + targrt.name)
  	    targrt.list.push(fn)
  	}
  	
  	//猎人工会走来了几个猎人
  	let hunterMing = new Hunter('小明', '黄金')
  	let hunterJin = new Hunter('小金', '白银')
  	let hunterZhang = new Hunter('小张', '黄金')
  	let hunterPeter = new Hunter('Peter', '青铜')
  	
  	//Peter等级较低，可能需要帮助，所以小明，小金，小张都订阅了Peter
  	hunterMing.subscribe(hunterPeter, function(money){
  		console.log('小明表示：' + (money > 200 ? '' : '暂时很忙，不能') + '给予帮助')
  	})
  	hunterJin.subscribe(hunterPeter, function(){
  		console.log('小金表示：给予帮助')
  	})
  	hunterZhang.subscribe(hunterPeter, function(){
  		console.log('小金表示：给予帮助')
  	})
  	
  	//Peter遇到困难，赏金198寻求帮助
  	hunterPeter.publish(198)
  	
  	//猎人们(观察者)关联他们感兴趣的猎人(目标对象)，如Peter，当Peter有困难时，会自动通知给他们（观察者）
  ```

二者的区别在于，观察者模式相当于是观察者实例直接订阅发布者的实例，发布者在发布后直接通知观察者。而订阅发布模式相当与有一个调度中心，发布者只将任务发布到调度中心，订阅者的信息也是直接保存在调度中心中，由调度中心发出指令。

### 3.4 vue 为什么无法监听数组/对象的变化

vue2使用的`Object.definePropertype`方法是通过对`data`进行递归遍历实现的对数据进行监控的，如果有一个很大的数组或者对象，遍历+递归的方式进行监听会非常的损耗性能，所以作者出于性能考虑，并没有对数组和对象进行深度监控，这就导致了在vue中某些情况下无法监听到数组和对象的改变。

* vue能监听数组改变的情况：

  1. 通过**赋值形式**改变的数组`let arr = [] arr = [1,2,3]`
  2. 通过`splice`方法改变的数组 `let arr = [1,2,3] arr.splice(0,1)`
  3. 通过`push`方法改变的数组`let arr = [1] arr.push(2)`

* vue无法监听的情况：

  * 数组

    1. 使用索引直接改变数组的某个值`let arr = [1,2,3] arr[0] = 3`
    2. 直接改变数组长度`let arr = [1,2,3,4] arr.length = 2`

  * 对象

    1. vue中只有在data中声明的对象属性是响应式的，后添加的都不是响应的。
    2. 在方法中对data中的对象进行增加，删除，改变，无法被vue监听到。

    ```javascript
    // data
    data () {
        return {
            obj:{
                name:'王大宝',
                age:2
            }
        }
    }
    
    // methods
    this.obj.sex = 'princess' // 非响应
    delete obj.age // 非响应
    this.obj.age = 0 // 响应，因为在data中声明了age
    this.obj.sex = 'girl' // 非响应，因为是在方法中添加的sex属性
    ```

    

* 无法监听情况的解决方案

  * 数组
    1. 使用`Vue.set()`

      ```javascript
      let arr = [1,2,3,4]
      Vue.set(arr,0,3)
      console.log(arr) // [3,2,3,4]
      ```
  
    2. 利用临时变量传参`let a = [1,2,3,4] let b = [...a] b[0] = 10 a = b` 注意，临时变量一定要是深拷贝出来的，重新开辟内存，不然不生效的。
    
    3. 使用`split()`方法对数组进行增加或删除。
    
  * 对象
  
    1. 使用`vue.set`
    2. 使用临时变量传参，同样的，临时变量一定要深拷贝出来，重新开辟内存。

vue3中使用了`proxy`方法代替了`Object.difinePropertype`，`Object.difinePropertype`方法的局限性在于它只能对单例属性进行监听，而`proxy`属性相当于对对象架设一层拦截，对目标对象的所有访问，都必须通过这层拦截，使用这个方法可以解决使用`Object.difinePropertype`所带来的的性能损耗问题，`proxy`的缺点是这个方法是ES6的方法，对旧的浏览器的支持不太好，所以在vue2中没有使用这种方法。

### 3.5 vue生命周期

![LifeRound](https://gitee.com/dqtwdd/img/raw/master/LifeRound.png)

#### 3.5.1 vue生命周期

1. `beforeCreated()`
   在实例初始化，完成创建之前调用
2. `created()`
   实例已经创建完成之后被调用。在这一步，实例已完成以下的配置：数据观测(data observer)，属性和方法的运算， watch/event 事件回调。然而，挂载阶段还没开始，$el 属性目前不可见。
3. `beforeMount()`
   在挂载开始之前被调用。 相关的 render 函数首次被调用。
4. `mounted()`
   el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用该钩子。
5. `beforeUpdate()`
   数据更新时调用，发生在虚拟 DOM 重新渲染和打补丁之前。 你可以在这个钩子中进一步地更改状态，这不会触发附加的重渲染过程。
6. `updated()`
   由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。
   当这个钩子被调用时，组件 DOM 已经更新，所以你现在可以执行依赖于 DOM 的操作。然而在大多数情况下，你应该避免在此期间更改状态，因为这可能会导致更新无限循环。
   该钩子在服务器端渲染期间不被调用。
7. `beforeDestroy()`
   实例销毁之前调用。在这一步，实例仍然完全可用。
8. `destroyed()`
   Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。 该钩子在服务器端渲染期间不被调用。

#### 3.5.2 ` activated `与`deactivated`

页面第一次进入，钩子的触发顺序`created -> mounted -> activated`，退出时触发`deactivated`。当再次进入（前进或者后退）时，只触发`activated`。

事件挂载的方法等，只执行一次的放在` mounted `中；组件每次进去执行的方法放在 `activated` 中， `activated` 中的不管是否需要缓存多会执行。

所以当页面设置了`keepalive`的时候，要想对页面数据进行更改，则可在`activated`中调用组件中相关的方法。调用方式和`mounted`一样。

#### 3.5.3 父子组件生命周期调用顺序

- 加载渲染过程 { } { ( ) ( ) }

  `父beforeCreate -> 父created -> 父beforeMount -> 子beforeCreate -> 子created -> 子beforeMount -> 子mounted -> 父mounted`

- 子组件更新过程 { ( ) }

  `父beforeUpdate -> 子beforeUpdate -> 子updated -> 父updated`

- 父组件更新过程 { }

  `父beforeUpdate -> 父updated`

- 销毁过程 { ( ) }

  `父beforeDestroy -> 子beforeDestroy -> 子destroyed -> 父destroyed`

练习：祖父 -> 父 -> 子

* 加载渲染过程 { } { [ ] [ ( ) ( ) ] }
* 子更新过程 { [ ( ) ] }
* 销毁过程 { [ ( ) ] } 

### 3.6 filter、computed和watch

* watch监控现有的属性,computed通过现有的属性计算出一个新的属性。
* watch不会缓存数据，每次打开页面都会重新加载一次。
* computed会缓存数据，所以computed的性能比watch更好一些。

#### 3.6.1 filter 

filter无缓存，调用频率高，因此比较适合在格式化输出的场景，主要在模板渲染时使用，主要作用是在数据展示之前对数据进行处理，让展示的数据符合我们的需求。filter支持链式调用。

比如：将要展示的字段全部小写/大写；把要展示的数字四舍五入/保留两位小数等；为字段添加单位等等。

示例如下：

```vue
<div>
    {{ item.age | ageFilter }}
</div>
<script>
export default {
  filters: {
    ageFilter: function(item) {
      return item + "岁";
    }
  }
};
</script>
```

#### 3.6.2 computed

 computed 属性具有缓存能力，在组件内普适性更强，因此适用于复杂的数据转换、统计等场景。 

```vue
<template>
  <div id="app">
    <div id="nav">
      <div @click="addAge" v-for="(item, index) in listComputed" :key="index">
        {{ item.age | ageFilter }}
        {{ item.age | ageFilter }}
      </div>
    </div>
  </div>
</template>
<script>
export default {
  computed: {
    listComputed: function() {
      let newArr = this.list.filter(item => {
        return item.age > 3;
      });
      return newArr;
    }
  }
};
</script>
```

#### 3.6.3 watch

watch主要用于监听某个vue中定义的属性， 一旦属性发生了改变就去自动调用对应的方法 。

### 3.7 路由守卫

#### 3.7.1 全局守卫

全局守卫包含**beforeEach、beforeResolve、afterEach**。

`beforeResolve`在`afterEach`之前调用。

`afterEach`在进入路由后调用。

全局守卫定义在`router.js`中，路由中的每一个组件都会通过全局守卫。可以用于权限判断等：

```javascript
const router = new VueRouter({ ... })

router.beforeEach((to, from, next) => {
  if (to.name !== 'Login' && !isAuthenticated) next({ name: 'Login' })
  else next()
})
// 以上代码实现了在所有页面进入之前判断是否有登录权限，如果没有直接跳转登录页。
```

#### 3.7.2 组件内守卫

组件内守卫包含**beforeRouterEnter、beforeRouteUpdate 、beforeRouterLeave**。

组件内守卫就是定义在组件内部的守卫，可以在`compuents`中定义使用：

```javascript
beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
},
beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
},
beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
}
```

`beforeRouteUpdate `在路由跳转，但是该组件被复用时调用 (2.2+) 。

`beforeRouterLeave`通常在用户即将离开某个页面时，用来提醒用户之前的操作不会被保存。

#### 3.7.3 独享守卫

独享守卫只有**beforeEnter**。

独享守卫写在`router`的路由对象中，为跳转某个路由独享的守卫：

```vu
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...
      }
    }
  ]
})
```



独享守卫和路由内守卫的主要区别在于组件内守卫在多个路由使用同一组件时都会调用，独享守卫只在进入某个路由时才会调用。

路由守卫调用顺序：

1. 调用**路由内守卫**离开页面的`beforeRouterLeave`
2. 调用**全局守卫**`beforeEach`
3. 如果新进入的路由组件复用的话，调用**路由内守卫**`beforeRouterUpdate`
4. 调用**独享守卫**`beforeEnter`
5. 调用**路由内守卫**`beforeRouterEnter`
6. 调用**全局守卫**`beforeResolve`
7. 调用**全局守卫**`afterEach`

### 3.8 VueX

#### 3.8.1 知识点

VueX拥有五个属性，分别是：`state getter mutation action module`

* `state`

  `state`可以理解为VueX的仓库，所有需要全局状态管理的数据都存放在`state`中，在组件中读取`state`值的方法有两种，一种是使用`getter`，另一种是使用VueX的`mapState`方法。使用`mapState`的优势在于，在某个组件中需要一次性使用大量VueX中的数据时，我们可以直接通过这种映射的方式把所有需要的属性都取过来，而不用一个一个去取。

* `getter`

  可以理解为VueX中的计算属性，它是VueX对外暴露的接口，可以将`state`中的值经过处理后返回。

* `mutation`

  VueX中有两种改变`store`中的值，一种是使用`this.$store.state.xxxx = 'xxx'`来改变（这种方法改变`state`中的值dom中可能不会同步更新），另一种是使用`mutation`改变，一般来说，VueX中的值不允许直接改变，只能使用`mutation`中的方法改变`state`中的值，这样可以方便追踪数据流向，使用方法为在组件的方法中调用`this.$store.commit('xxx')`：

  ```javascript
  // store.js
  import Vue from "vue";
  import Vuex from "vuex";
  
  Vue.use(Vuex);
  
  export default new Vuex.Store({
    state: {
      daughter: "王一一",
      age: 2,
      father: "栋栋",
      mother: "董菜菜",
      toys: ["小飞机", "非洲鼓", "小钢琴"]
    },
    getters: {
      getAge: function(state) {
        return state.age + "岁";
      }
    },
    mutations: {
      addAge(state, addNum = 1) {
        state.age += addNum;
      }
    },
    actions: {
        yearGone(content, year = 1) {
        return new Promise(resolve => {
          console.log("马上要执行+1啦");
          content.commit("addAge", year);
          console.log("+1完成啦");
          resolve;
        });
    },
    modules: {}
  });
  
  // app.vue
  <template>
      <div @click="print"></div>
  </template>
  <script>
  import { mapState } from "vuex";
  export default {
    computed: {
      ...mapState(["daughter", "age"])
    },
    data() {
      return {};
    },
    methods: {
      ...mapActions(["yearGone"]),
      print() {
        this.$store.commit("addAge");
      },
      printAsync() {   
        this.yearGone(2).then(() => {
          console.log("actionsssssssssssss执行完啦！");
        });
      },
    }
  };
  </script>
  // printAsync : 马上要执行+1啦 +1完成啦 actionsssssssssssss执行完啦!
  ```

* `action`

  `action`的主要魅力在于`action`支持异步，在某些情况下，我们需要确定`mutation`改变`state`中的值之后再进行下一步操作，这时就需要用到`actions`。

  

* `module`

  在项目开发过程中，随着功能的增加，一个`VueX`里面储存的变量可能变得非常多，此外，如果多人开发一个项目，在项目开发时可能会出现`state mutation getters actions`重名的现象，这时，我们就用到了`module`。

  首先要说的是，`module`可以将`VueX`分解成多个`store`：

  ```javascript
  import Vue from "vue";
  import Vuex from "vuex";
  Vue.use(Vuex);
  const father = {
    state: {
      fatherName: "栋栋",
      fatherAge: 27
    },
    getters: {
      getFatherAge: function(state) {
        return "爸爸" + state.age + "岁";
      }
    },
    mutations: {
      fatherAddAge(state, addNum = 1) {
        state.age += addNum;
      }
    }
  };
  const mother = {
    namespaced: true,
    state: {
      name: "雨雨",
      age: 26
    },
    getters: {
      getAge: function(state) {
        return "妈妈" + state.age + "岁";
      }
    },
    mutations: {
      addAge(state, addNum = 1) {
        state.age += addNum;
      }
    }
  };
  const daughter = {
    namespaced: true,
    state: {
      name: "一一",
      age: 2
    },
    getters: {
      getAge: function(state) {
        return "大闺女" + state.age + "岁";
      }
    },
    mutations: {
      addAge(state, addNum = 1) {
        state.age += addNum;
      }
    }
  };
  export default new Vuex.Store({
    modules: {
      father,
      mother,
      daughter
    }
  });
  ```

  上面的代码中，`state`可以使用`this.$store.father.fatherName this.$store.mother.name this.$store.daughter.name`来获取。`father`定义的属性和另外两个都不一样，所以可以在组件中直接通过`this.$store.commit('fatherAddAge')   this.$store.getters.getFatherAge`来触发`father`中的方法，`mother`和`daughter`定义的属性是重复的，这时最好为`modules`添加`namespaced `属性，来区分各个包的方法，方法为`this.$store.commit('mother/addAge') this.$store.commit('daughter/addAge') this.$store.getters["mother/getAge"] this.$store.getters["daughter/getAge"]`。

* 辅助函数 （` mapState mapGetters mapActions mapMutations `）

  ` mapState mapGetters`写在`computed`中，` mapActions mapMutations`写在`method`中，都是相当于把要调用的值/方法直接映射在组件中。

  ```vue
  <template>
    <div></div>
  </template>
  <script>
  import { mapState, mapGetters, mapActions, mapMutations } from "vuex";
  export default {
    data() {
      return {};
    },
    computed: {
      ...mapState({
        viewsCount: "views"
      }),
      ...mapGetters({
        todosALise: "getToDo" // getToDo 不是字符串，对应的是getter里面的一个方法名字 然后将这个方法名字重新取一个别名 todosALise
      })
    },
    methods: {
      ...mapMutations({
        totalAlise: "clickTotal" // clickTotal 是mutation 里的方法，totalAlise是重新定义的一个别名方法，本组件直接调用这个方法
      }),
      ...mapActions({
        blogAdd: "blogAdd" // 第一个blogAdd是定义的一个函数别名称，挂载在到this(vue)实例上，后面一个blogAdd 才是actions里面函数方法名称
      })
    }
  };
  </script>
  ```

  

#### 3.8.2 常见面试题

1. vuex有哪几种属性？ 

   有五种，分别是 State、 Getter、Mutation 、Action、 Module

2. vuex的State特性是？

   1. Vuex就是一个仓库，仓库里面放了很多对象。其中state就是数据源存放地，对应于与一般Vue对象里面的data
   2. state里面存放的数据是响应式的，Vue组件从store中读取数据，若是store中的数据发生改变，依赖这个数据的组件也会发生更新 三、它通过mapState把全局的 state 和 getters 映射到当前组件的 computed 计算属性中 

3. vuex的Getter特性是？ 

   1. getters 可以对State进行计算操作，它就是Store的计算属性 
   2. 虽然在组件内也可以做计算属性，但是getters 可以在多组件之间复用
   3. 如果一个状态只在一个组件内使用，是可以不用getters 

4. vuex的Mutation特性是？

   Action 类似于 mutation，不同在于：Action 提交的是 mutation，而不是直接变更状态。Action 可以包含任意异步操作 

5. Vue.js中ajax请求代码应该写在组件的methods中还是vuex的actions中？ 

   1. 如果请求来的数据是不是要被其他组件公用，仅仅在请求的组件内使用，就不需要放入vuex 的state里。
   2. 如果被其他地方复用，这个很大几率上是需要的，如果需要，请将请求放入action里，方便复用，并包装成promise返回，在调用处用async await处理返回的数据。如果不要复用这个请求，那么直接写在vue文件里很方便。 

6. 不用Vuex会带来什么问题？ 

   1. 可维护性会下降，你要想修改数据，你得维护三个地方
   2. 可读性会下降，因为一个组件里的数据，你根本就看不出来是从哪来的
   3. 增加耦合，大量的上传派发，会让耦合性大大的增加，本来Vue用Component就是为了减少耦合，现在这么用，和组件化的初衷相背。 但兄弟组件有大量通信的，建议一定要用，不管大项目和小项目，因为这样会省很多事  

### 3.9 如何实现一个vue插件

### 3.10 vue-router原理

首先，从在浏览器地址栏输入地址到返回页面经历以下几个步骤：

1. dns解析服务器地址。
2. 三次握手简历tcp链接。
3. 发送http请求到服务端。
4. 服务器处理http请求。
5. 服务器返回请求结果。
6. 关闭tcp链接。
7. 浏览器解析返回结果。
8. 浏览器布局渲染页面。

* 后端路由 后端路由相当于每一次改变地址（path）之后都会重新经历上面的流程，都重新去获取页面资源并渲染页面。
* 前端路由 前端路由，特别是现在一般使用的单页面应用框架，一般只需要第一次请求页面信息，然后都是在同一个html中渲染不同的内容。在路由改变时，是由框架的js监听地址的变化，然后通过js给变页面内容。主要是以下一个步骤：
  1. 输入url
  2. js解析地址
  3. 找到地址对应的页面
  4. 行页面的js
  5. 渲染页面

此外前端路由也可以分为两种模式，一种是hash模式，一种是history模式；两种模式最大的区别在于history模式的可读性更好，更美观。在使用history模式时需要后台设置，将所有路由默认重定向到根页面，不然就会出现404错误。

### 3.11 vue keep-alive原理

首先，keep-alive是一个抽象组件，它本身不会渲染dom元素，也不会出现在父组件链中，使用keep-alive包裹动态组件时，会缓存不活动的组件实例，而不是销毁他们。

### 3.12 vue directive (自定义指令)

自定义指令是用来操作DOM的，尽管Vue推崇数据驱动识图的理念，但并非所有的情况都适合数据驱动。自定义指定就是一种有效的补充和扩展，不紧可以用于定义任何dom的操作，并且是复用的。

vue 指令的钩子函数

* bind：只调用一次，指令第一次绑定到元素时调用，这里可以进行一次性的初始化设置。
* inserted：被绑定的元素插入父节点时调用（保证父节点存在，但不一定已被插入文档中）。
* update：所在组件的VNode更新时调用，但是可能发生在其子VNode更新之前。指令的值可能发生了改变，也可能没有。但是你可以通过比较更新前后的值来忽略不必要的模板更新。
* componentUpdated：指令所在组件的VNode及其子VNode全部更新后调用。
* unbind：调用一次，指令与元素解绑时调用。

钩子函数的参数：

* el：指令绑定的元素，可以用来直接操作dom。
* binding：一个对象，包含以下property：
  * name：指令名，不包括v-前缀
  * value：指令的绑定值，例如：v-my-directive="1+1"中，绑定值为2。
  * expression：字符串形式的指令表达式。例如v-my-directive="1+1"中，指令表达式为"1+1"
  * arg：传给指令的参数，可选，例如：v-my-directive:foo中，参数为"foo"。
  * modifiers：一个包含修饰符的对象。例如：v-my-directive.foo.bar中，修饰符对象为{foo:true,bar:true}。
* vnode：vue编译生成的虚拟节点。
* oldVNode：上一个虚拟节点，仅在update和componentUpdated钩子中可用。

## 4.webpack相关知识

### 4.1 webpack与glup区别

webpack是一个模块化打包工具，他可以将各种格式的文件通过loader（加载器）和plugin（插件）对资源进行处理，打包成符合环境部署的前端资源。webpack强调的是模块化打包。
webpack is a module bundle.

loader : sass-loader、svg-sprite-loader等

pugin : DllPlugin  DllReferencePlugin speed-measure-webpack-plugin webpack-bundle-analyzer

glup强调的是前端开发的工作流程，我们可以通过配置一系列的task（比如压缩文件合并，雪碧图，版本控制等）然后定义执行顺序，然后让glup执行这些task，从而构建整个项目。glup强调的是开发流程。
glup is a task Runner.

### 4.2 webpack原理

webpack打包原理：当webpack处理程序时，会根据文件间的依赖关系对其进行静态分析，根据依赖关系递归的构建一个依赖关系图（dependency graph），然后将这些模块按照指定的规则生成静态资源，其中包含应用程序需要的每个模块，然后将这些模块打包程一个或多个bundle。

## 5. XSS

## 6.数据结构

### 6.1 二叉树

二叉树是一种典型的树状结构，有根、左子树和子右子树。

前序遍历 根左右  中序遍历 左根右  后序遍历 左右根

只要有第一个，就一直找第一个。

下图中        前序：ABCDEGF   中序：CBEGDFA  后序：CGEFDBA

![BinaryTree](https://gitee.com/dqtwdd/img/raw/master/BinaryTree.png)

### 6.2 链表

链表的就是用一组任意储存单元来储存线性表的数据结构，每一个节点对象储存着它本身的值和他指向的下一个节点的地址。

链表分 线性链表，双向链表，环形链表。



## 7. 算法

### 7.1 数组扁平化

### 7.2 动态规划

### 7.3 获取地址栏中所有参数

## 8. 有哪些项目优化方法

### 8.1 加快HTML页面加载速度

1. 页面减肥

   * 删除不必要的空格、注释。
   * 将inline的script和css移到外部文件。

2. 减少文件数量

   * 减少页面上引用的文件数量可以减少HTTP链接数。
   * 许多JavaScript、CSS文件可以合并最好合并。

3. 减少域名查询

   DNS查询和解析也是消耗时间的，所以要减少对外部JavaScript、CSS图片等资源的引用，不同域名的使用越少越好。

4. 缓冲重用数据

   * 对重复使用的数据进行缓存

5. 优化页面元素的加载顺序

   * 首先加兹安页面最初显示的内容与之相关的JavaScript和CSS
   * 然后加载HTML相关的东西，像什么不是最初显示相关的图片，视频等很肥的资源就最后加加载。

6. 指定图像和table的大小

   如果浏览器可以立即决定图像或者table的大小，那么他就可以马上显示而不需要重新做一些布局安排工作。这不仅加快了页面显示，也预防了页面完成加载后布局的一些不当的改变。

## 9. 工作中遇到的兼容性问题



