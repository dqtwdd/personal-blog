---
title: 滑动窗口
tags: ['学习']
categories: 学习
date: 2021-09-26
---

本文简单解释一下滑动窗口。

参考文档：[你还在为 TCP 重传、滑动窗口、流量控制、拥塞控制发愁吗？](https://juejin.cn/post/6854573218683387917#heading-1)

<!--more-->

先看一个三次握手的动图：

![tcp](https://gitee.com/dqtwdd/img/raw/master/tcp.awebp)

先简单说一下这几个参数的意义：

SYN：同步序列编号（ Synchronize Sequence Numbers ）。

* 是TCP/IP建立连接时使用的握手信号。
* 在客户机和 服务器 之间建立正常的TCP网络连接时，客户机首先发出一个SYN消息，服务器使用SYN+ACK应答表示接收到了这个消息，最后客户机再以 ACK 消息响应。
* 这样在 客户机 和服务器之间才能建立起可靠的TCP连接，数据才可以在客户机和服务器之间传递。

ACK: 确认字符 (Acknowledge character）。

* 在数据通信中，接收站发给发送站的一种传输类控制字符。表示发来的数据已确认接收无误。在TCP/IP协议中，如果接收方成功的接收到数据，那么会回复一个ACK数据。



数据本身是有顺序的，tcp链接的特点就是可靠。可靠，就指的是发送的数据每个都会被接受，并且是按顺序的，不会丢包，这时我们就会有一个疑问：

### 如何保证数据传输的次序？

简单的说就是主机A先发送一段数据，然后主机B收到之后返回一个ACK（确认字符），然后主机A收到之后再发送下一段数据。

因为每次发送数据都需要接收的主机返回ACK，这样虽然解决了丢包、出错、乱序等问题，但是这样也出现了新问题：吞吐量低。

正常数据传输的过程如下：

![sendData](https://gitee.com/dqtwdd/img/raw/master/sendData.awebp)

### 如何提高吞吐量？

提高吞吐量，就很自然的回到我们本文的重点：滑动窗口。

![slidingWindow](https://gitee.com/dqtwdd/img/raw/master/slidingWindow.awebp)

如上图，我们已经完成了1/2/3三段数据的传输，当前窗口处于4-10，其中4/5/6/7已经发送，8/9/10还未发送。

这个滑动窗口的大小是可以变化的，且滑动窗口的大小由服务端能接受的大小来控制。

当4号数据被接收并且返回确认字符之后，窗口就像后滑动一格，将11放入待发送队列。如下图：

![slidingWindow1](https://gitee.com/dqtwdd/img/raw/master/slidingWindow1.awebp)

正常情况的滑动窗口就是这样工作了，但是我们可以发现一个新的问题：

窗口的滑动是由窗口内第一个数据是否接收到确认字符来决定的，也就是说，5-11这段窗口内6-11的数据全部发送完成，并且接收到确认字符，但是数据5发生了丢包或者确认字符丢失，整个窗口就滑不动了，数据传输陷入了瘫痪，这显然是我们不能接受的，该如何解决呢？

### 滑动窗口的重传机制

1. 超时重传

   超时重传的核心就是设置一个重传时间，一个数据发送后，一旦时间超过重传时间并且没有收到ACK返回，就立即重新发送请求。

   如何配置重传时间也有其一套合理的算法，具体算法可以查看本文参考。

2. 快速重传

   快速重传就是将重传的标准由时间改为接收ACK的顺序。

   比如，上图中5-11段的数据都已经发送，但是数据5/7的数据没有收到ACK，但是后发送的11的数据都已经收到了ACK，我们就可以直接重新发送5/7两段数据。

3. SACK（ Selective Acknowledgment 选择性确认）

   SACK就是在TCP头部选项里加一个SACK字段，将哪段数据收到哪段数据没收到数据跟随ACK返回数据发送方，这样发送方就知道该发送哪段数据。

4. D-SACK（Duplicate SACK 二重的选择性确认）

   D-SACK和SACK相比可以将重复接收的数据返回告诉发送端，让发送方知道是发出去的包丢了，还是接收方的ACK包丢了。可以知道是不是发送方的数据包被网络延迟了。等等。

   
