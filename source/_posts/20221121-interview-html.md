---
title: 面试准备
tags: ['工作']
categories: 工作
date: 2021-01-06
---

记录工作中想到的面试应该会的问题。

<!--more-->

## 1. HTML + CSS

### 1.1 HTTP 状态码及其含义

- 1XX：信息状态码
  - 100 Continue 继续，一般在发送 post 请求时，已发送了 http header 之后服务端将返回此信息，表示确认，之后发送具体参数信息
- 2XX：成功状态码
  - 200 OK 正常返回信息
  - 201 Created 请求成功并且服务器创建了新的资源
  - 202 Accepted 服务器已接受请求，但尚未处理
- 3XX：重定向
  - 301 Moved Permanently 请求的网页已永久移动到新位置。
  - 302 Found 临时性重定向。
  - 303 See Other 临时性重定向，且总是使用 GET 请求新的 URI。
  - 304 Not Modified 自从上次请求后，请求的网页未修改过。
- 4XX：客户端错误
  - 400 Bad Request 服务器无法理解请求的格式，客户端不应当尝试再次使用相同的内容发起请求。
  - 401 Unauthorized 请求未授权。
  - 403 Forbidden 禁止访问。
  - 404 Not Found 找不到如何与 URI 相匹配的资源。
- 5XX: 服务器错误
  - 500 Internal Server Error 最常见的服务器端错误。
  - 503 Service Unavailable 服务器端暂时无法处理请求（可能是过载或维护）。

### 1.2 如何进行网站性能优化

1. 页面懒加载。
2. 使用 webpack 插件（terser-webpack-plugin），在打包时删除冗余代码 。
3. 使用**SplitChunks** 手动将一些复用性高的文件抽离出来。
4. 使用 CDN。
5. 将较大的图片进行无损压缩。

### 1.3 H5 新增的特性(只列出了常用的几个)

1. 新增语义化标签 `header nav footer section`
   
   ```javaScript
   <header> :头部标签
   <nav> : 导航标签
   <footer> :尾部标签
   <section> :块级标签
   ```

2. 新增多媒体标签 `audio video `
   
   ```javascript
   <audio src="MP3.mp3" controls="controls">您的浏览器不支持Audio元素</audio>
   
   video元素要设定好长宽和src属性就可以了：
   
   <video width="750" height="400" src="time.mp4"></video>
   ```

3. 新增绘图功能 ` canvas svg`

### 1.4 ` <!DOCTYPE HTML>`的作用

声明文档类型为 HTML

### 1.5 cookies，sessionStorage 和 localStorage 的区别

- cookie 是网站为了标示用户身份而储存在用户本地终端（Client Side）上的数据（通常经过加密），一般用来携带
- cookie 数据始终在同源的 http 请求中携带（即使不需要），记会在浏览器和服务器间来回传递
- sessionStorage 和 localStorage 不会自动把数据发给服务器，仅在本地保存
- 存储大小：
  - cookie 数据大小不能超过 4k
  - sessionStorage 和 localStorage 虽然也有存储大小的限制，但比 cookie 大得多，可以达到 5M 或更大
- 有效时间：
  - localStorage 存储持久数据，浏览器关闭后数据不丢失除非主动删除数据
  - sessionStorage 数据在当前浏览器窗口关闭后自动删除
  - cookie 设置的 cookie 过期时间之前一直有效，即使窗口或浏览器关闭

### 1.6 iframe

iframes 提供了一个简单的方式把一个网站的内容嵌入到另一个网站中。

缺点：

- iframe 会阻塞主页面的 Onload 事件
- 搜索引擎的检索程序无法解读这种页面，不利于 SEO

### 1.7 行内元素有哪些？块级元素有哪些？ 空(void)元素有那些？行内元素和块级元素有什么区别？

- 行内元素有：a b span img input select strong
- 块级元素有：div ul ol li dl dt dd h1 h2 h3 h4…p
- 空元素：`<br> `
- 行内元素不可以设置宽高，不独占一行
- 块级元素可以设置宽高，独占一行

### 1.8 display: none;与 visibility: hidden;的区别

- 联系：它们都能让元素不可见

- Vue 的`v-show`相当于`display: none` 而 `v-if`是不在 dom 中渲染该节点

- 区别：

- - display:none;会让元素完全从渲染树中消失，渲染的时候不占据任何空间；visibility: hidden;不会让元素从渲染树消失，渲染师元素继续占据空间，只是内容不可见
  - display: none;是非继承属性，子孙节点消失由于元素从渲染树消失造成，通过修改子孙节点属性无法显示；visibility: hidden;是继承属性，子孙节点消失由于继承了 hidden，通过设置 visibility: visible;可以让子孙节点显式
  - 修改常规流中元素的 display 通常会造成文档重排。修改 visibility 属性只会造成本元素的重绘。
  - 读屏器不会读取 display: none;元素内容；会读取 visibility: hidden;元素内容

### 1.9 清除浮动

清除浮动的几种方式，各自的优缺点

- 使用空标签清除浮动 clear:both（缺点，增加无意义的标签）

- 使用 overflow:auto（使用 zoom:1 用于兼容 IE，缺点：内部宽高超过父级 div 时，会出现滚动条）

- 用 afert 伪元素清除浮动(IE8 以上和非 IE 浏览器才支持。
  
  ```html
  // dom
  <div class="main">
    <div class="box blue"></div>
  </div>
  .main { border:20px solid blueviolet; } .main::after { content:"";
  display:table; /*采用此方法可以有效避免浏览器兼容问题*/ clear:both; } .box {
  width: 100px; height: 100px; } .blue{ background-color: blue; float: left; }
  //css
  ```

### 1.10 css3 有哪些新特性

- 新增各种 css 选择器
  
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
  
  **ps 一个坑：伪类选择器，比如`last-child `或者`last-of-type` 的意思是，既要是最后一个，还得是指定的类型**
  
  [示例链接](https://blog.csdn.net/Dorothy1224/article/details/95206594)

- flex 布局

- 圆角 `border-radius`

- 阴影和反射`shadow`

- 文字特效`text-shadow`

- 线性渐变`linear-gradient(#fff2d9, #fff2d9 50%, #f9e7b9 51%, #f9e7b9)`

- 旋转`transform` 包含`scale (缩放) rotate (旋转) skew (倾斜)`

- 媒体查询`@media only screen and (min-width: 1200px) {}`

- 过度`transition`

- 动画`animation`

### 1.11 display 有哪些值？说明他们的作用

- block 象块类型元素一样显示。
- inline-block 象行内元素一样显示，但其内容象块类型元素一样显示。
- list-item 象块类型元素一样显示，并添加样式列表标记。
- table 此元素会作为块级表格来显示
- inherit 规定应该从父元素继承 display 属性的值

### 1.12 介绍一下标准的 CSS 的盒子模型？低版本 IE 的盒子模型有什么不同的？

有两种， IE 盒子模型、W3C 盒子模型；内容(content)、填充(padding)、边框(border)、边界(margin) ；

- 盒模型(box-sizing:content-box): width/height 为 content 的宽高。
- 怪异模型(box-sizing:border-box)：width/height 为 content + padding + border 的宽高。

### 1.13 position 的值， relative 和 absolute 定位原点是

- absolute：生成绝对定位的元素，相对于 static 定位以外的第一个父元素进行定位。
- fixed：生成绝对定位的元素，相对于浏览器窗口进行定位。
- relative：生成相对定位的元素，相对于其正常位置进行定位。
- static 默认值。没有定位，元素出现在正常的流中。
- inherit 规定从父元素继承 position 属性的值。

### 1.14 display:inline-block 出现缝隙解决方案

- 使用 margin 负值（大概-5px）
- 使用 font-size:0

### 1.15 从输入网址，到页面展示，浏览器做了什么

1. DNS 解析 ip 地址。
2. 三次握手建立 Tcp 链接。
3. 发送 Http 请求。
4. 服务器处理请求。
5. 服务器返回响应结果。
6. 关闭 Tcp 链接。
7. 浏览器解析 HTML。
8. 浏览器布局渲染页面。

[细说浏览器输入 URL 后发生了什么](https://segmentfault.com/a/1190000012092552)

### 1.16 请求头的作用

请求头主要用来携带客户端向服务器发送请求的一些描述，比如 http 协议类型，客户端所接受的编码方式，发送内容的长度等；某些用户信息也携带在请求头中，比如说 cookie，token 等；还携带了 referer 信息。

referer 信息代表当前请求的源头，可以用来防止盗链，防止恶意请求。

响应头主要用来携带服务器返回的 content 的一些描述，比如编码类型，内容长度等。

### 1.17 同源策略

浏览器中的**协议、域名、端口号**相同则属于同源。

同源策略是浏览器的一个安全功能，不同源的客户端脚本在没有明确授权的情况下，不能读写对方资源。

什么要有同源策略：主要是为了保护用户的网络安全。禁止对不同源的 DOM 进行访问，禁止向不同源的服务器发起 HTTP 请求。

### 1.18 几种常用 css 布局

#### 1.18.1 居中

- 文字居中
  
  - 垂直居中
    
    1. 为文字添加相等的 padding-top 和 padding-bottom
    2. 单行文字且不会换行展示使`line-height`与容器`height`相等
  
  - 水平居中
    
    设置`text-aligin:center`

- `dom`居中
  
  1. 绝对定位配合`transform`居中
     
     ```css
     .center {
       left: 50%;
       right: 50%;
       transform: translate(-50%, -50%);
     }
     ```
  
  2. 使用`flex`布局
     
     ```css
     .center {
       display: flex;
       justify-content: center;
       align-items: center;
     }
     ```

#### 1.18.2 两栏布局

1. BFC 实现两栏布局
   
   ```html
   <!--html-->
   <div class="container">
     <div class="left">左边左边左边左边左边左边左边左边左边左边</div>
     <div class="right">
       右边右边右边右边右边右边右边右边右边右边右边右边右边右边右边右边右边
     </div>
   </div>
   <!--css-->
   .container { width:100%; } .left {
   <!--!important-->
   float:left;
   <!--!important-->
   width:100px; background-color: red; } .right {
   <!--!important-->
   overflow:hidden;
   <!--!important-->
   background-color: blue; min-height:100vh; }
   ```

2. flex 实现两栏布局
   
   ```html
   <!--html-->
   <div class="container">
     <div class="left">左边左边左边左边左边左边左边左边左边左边</div>
     <div class="right">
       右边右边右边右边右边右边右边右边右边右边右边右边右边右边右边右边右边
     </div>
   </div>
   <!--css-->
   .container { width:100%;
   <!--!important-->
   display:flex;
   <!--!important-->
   } .left { width:100px; background-color: red; } .right {
   <!--!important-->
   flex:1;
   <!--!important-->
   background-color: blue; }
   ```

### 1.19 BFC（Block-Formatting-Context 块格式化上下文）

- 作用：
  
  1. 解决外边距合并问题。
     
     ```html
     <!-- HTML -->
     <div class="container">
       <div class="box red"></div>
       <div class="box blue"></div>
     </div>
     <!-- CSS -->
     .container { position: absolute; border: 5px solid lightcoral; } .box {
     width:100px; height:100px; padding:100px; } .red { background-color:red; }
     .blue { background-color:blue; }
     ```
     
     ![OuterMargin](https://dqtwdd.top/cdn/img/OuterMargin.jpg)
     
     以上代码实际展示出来是这个样子的，可以很明显的看出来了两个块之间的间距只有 100px，使用 BFC 的解决办法就是在两个块外层加一个 BFC 的容器。
     
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
     .container { position: absolute; border: 5px solid lightcoral; } .bfc {
     overflow:hidden } .box { width:100px; height:100px; } .red {
     background-color:red; } .blue { background-color:blue; }
     ```
     
     ![OuterMarginAfterBFC](https://dqtwdd.top/cdn/img/OuterMarginAfterBFC.jpg)
  
  2. 解决 float 导致的高度塌陷问题，清除浮动。
     
     ```html
     <!-- HTML -->
     <div class="container">
       <div class="box red"></div>
     </div>
     <!-- CSS -->
     .container { border: 5px solid lightcoral; width: 100%; } .box { width:
     100px; height: 100px; } .red { background-color: red; float: left; }
     ```
     
     ![Float](https://dqtwdd.top/cdn/img/Float.jpg)
     
     ```html
     <!-- HTML -->
     <div class="container">
       <div class="box red"></div>
     </div>
     <!-- CSS -->
     .container { overflow: hidden; border: 5px solid lightcoral; width: 100%; }
     .box { width: 100px; height: 100px; } .red { background-color: red; float:
     left; }
     ```
     
     ![FloatAfterBFC](https://dqtwdd.top/cdn/img/FloatAfterBFC.jpg)
  
  3. 用来布局（比如经典的两栏布局）
     
     ```html
     <!-- HTML -->
     <div class="container">
       <div class="left"></div>
       <div class="right"></div>
     </div>
     <!-- CSS -->
     .container { width: 100%; } .left { float:left; min-height:100vh;
     background-color: red; } .right { background-color: blue; min-height:100vh;
     overflow: hidden; }
     ```
     
     ![TwoLayout](https://dqtwdd.top/cdn/img/TwoLayout.jpg)

- 形成 BFC 的条件
  
  1. 浮动元素，float 除 none 以外的值；
  2. 绝对定位元素，position（absolute，fixed）；
  3. display 为以下其中之一的值 inline-blocks，table-cells，table-captions；
  4. overflow 除了 visible 以外的值（hidden，auto，scroll）

### 1.20 实现一个三角形 css

```css
.triangle {
  height: 0;
  width: 0;
  border: 50px solid transparent;
  border-top-color: ;
}
```

### 1.21 tcp/udp

- tcp 全称 传输控制协议。是一种面向连接的，可靠的，基于字节流的传输通信协议。
  
  tcp 链接过程需要经过三次握手：
  
  1. 我要发请求了，ok 吗？（客户端向服务端发送请求报文段，客户端进入 SYN_SEND（请求链接）状态）
  2. ok！我准备好了，发吧！（服务端收到连接请求报文段后，如果同意链接，则发送一个应答，同时客户端进入 SYN_RECEIVE（准备接收）状态）
  3. ok，我也准备好了，这就发。（客户端收到链接同意的应答后，还要向服务器发送一个确认报文，发送完成后，客户端进入 ESTABLISHED（已连接）状态，服务端接收到请求后也进入 ESTABLISHED 状态，此时连接建立成功）
  
  然后就开始了。第三次握手是为了防止出现失效的链接请求报文被服务端接受的情况。
  
  断开链接过程需要经过四次握手：
  
  1. 我没啥要发的了，可以断开链接吗？（客户端进入 FIN_WAIT_1 状态）
  2. 我看看啊，要是没有了我告诉你。（此时，服务端进入 CLOSE_WAIT 状态，服务端已经不再接受客户端发送的数据，但是仍然可以发送数据给客户端，客户端手收到后进入 FIN_WAIT_2 状态）
  3. （如果有没发完的继续发送，发送完成后）我没啥发的了，你也断开吧。（此时，服务端进入 LAST_ACK 状态，等客户端发送 ACK 确认连接后就关闭链接）
  4. 好的，我也断开了。（此时客户端进入 TIME_WAIT 状态，在一段时间内没收到 B 的重发请求的话，就进入 CLOSED 状态，此条信息发送的就是 ACK 链接，B 收到确认应答后，就进入 CLOSED 状态）
  
  TCP 特点：
  
  - 面向连接 是指发送数据之前必须在两端建立链接。
  - 仅支持单播传输 每条 TCP 传输只能有两个端点，只能进行点对点的数据传输，不支持多播和广播的传输方式。
  - 面向字节流 TCP 不像 UDP 一样以一个一个报文独立的传输，而是在不保留报文边界的情况下以字节流方式进行传输。
  - 可靠传输 TCP 为了保证报文传输的可靠，给每一个包一个需要，同时序号也保证了传输到接受端实体包的按序接受，然后接收端对已经接收到的字节发挥一个相应的确认（ACK），如果发送端实体在合理往返延时内未收到确认，那么就会将对应的数据进行重新传送。

- UDP 全称用户数据报协议，是一种无连接协议。UDP 具有不提供数据包分组、组装和不能对数据包进行排序的缺点，也就是说，报文发送后，是无法得知是否完整到达的。

应用场景：文件传输等。

UDP 特点：

- 面向无连接。 不需要进行连接，想发数据就开始发送。
- 有单播，多播，广播的功能 UDP 不只支持一对一的传输方式，还支持一对多，多对多，多对一的方式。
- UDP 是面向报文的。
- 不可靠 不知道是否接受，是否有丢包。

应用场景：电话会议，视频等。

|        | UDP                     | TCP                 |
|:------ |:----------------------- | ------------------- |
| 是否连接   | 无连接                     | 面向连接                |
| 是否可靠   | 不可靠传输，不使用流量控制和拥塞控制      | 可靠传输，使用流量控制和拥塞控制    |
| 连接对象个数 | 支持一对一，一对多，多对一和多对多交互通信   | 只能是一对一通信            |
| 传输方式   | 面向报文                    | 面向字节流               |
| 首部开销   | 首部开销小，仅 8 字节            | 首部最小 20 字节，最大 60 字节 |
| 适用场景   | 适用于实时应用（IP 电话、视频会议、直播等） | 适用于要求可靠传输的应用，例如文件传输 |

### 1.22 html 加载顺序

- 下载的顺序就是从上到下，渲染的顺序也是从上到下，下载和渲染时同时进行的。
- 在渲染到页面的某一部分时，其上面所有部分都已经下载完成（并不是说所有相关联的元素都已经下载完）
- 如果遇到语义解释性的标签嵌入文件（Js 脚本，Css 样式），那么此时浏览器的下载过程会启用单独的链接进行下载。
- 样式表在下载完成后，将和以前下载的所有样式表一起进行解析，解析完成后，将对此前所有元素（含以前已经渲染的）重新进行渲染。
- Js、Css 中如果有重定义，后定义的函数将覆盖前定义的函数。

html：贯穿整个页面；

css（三种声明方式）：

1. 外联样式表：在 head 标签中，使用 link 标签的 href 属性来引用后缀名为.css 的 css 样式文件。
2. 内联样式表：在 head 标签下的 style 标签，选择器+样式声明。
3. 外部样式表：在标签的 style 属性中添加 css 样式声明。

JavaScript：在`<script>`标签中，可以在 head 标签汇总，也可以在 body 标签中。

运行顺序：从上到下运行。

先解析 head 标签中的代码：

head 标签会包含一些引用外部文件的代码，从开始运行就会下载这些被引用的文件。

当遇到`<script>`标签时，浏览器会暂停解析（下载不会暂停），将控制权交给 JavaScript 引擎（解释器）。

如果`<script>`标签引用了外部脚本，就下载该脚本，否则就直接执行，执行完成后将控制权交给浏览器渲染引擎。

当 head 中代码解析完毕，会开始解析 body 中的代码：

如果 head 中引用的外部文件没有下载完，将会继续下载。

此时如果遇到 body 标签中的`<script>`，同样会将控制权交给 JavaScript 引擎来解析 JavaScript。

解析完毕后将控制权交还给浏览器渲染引擎

当 body 中的代码全部执行完毕，并且整个页面的 css 样式加载完毕后，css 会重新渲染整个页面的 html。

# 
