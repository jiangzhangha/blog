---
title: 《TCP/IP详解》协议卷读书笔记之 TCP简介
date: 2020-08-25 20:50:00
categories: 网络
index_img: /img/articles/tcp_ip4.jpg
tags:
	- 网络
	- TCP
	- IP
---

本文是经典著作《TCP/IP详解》协议卷（卷一）的读书笔记，对第十二章TCP简介部分进行总结与梳理

<!-- more -->

## 简介

之前介绍的IP协议、UDP协议、以太协议等都不提供数据的可靠传输服务，虽然这些协议通过CRC等可以检测一些数据的错误，但是这些协议并不会努力的修复错误，如果传输不成功，这些协议只是简单的放弃数据传输。因此这些协议是不承诺数据的必达。

在各种网络条件下，传输介质可能会导致数据的丢失或者错误，这样的问题很容易出现，如何解决这样的问题，计算机科学家们作出了很多的努力。在理论上，由很多数学方法，可以检测数据的某些位发生了错误，并作出修正。但是这些算法比较复杂。解决传输错误的另一个简单的方法，就是**再传送一次**，这种方法也被称为`automatic repeat request`，即`ARQ`。TCP协议也是基于`ARQ`实现数据的可靠传输。

当我们考虑传输的通道不止一个时，就会发现实际情况下各种各样的错误都可能冒出来，比如数据包接收的顺序与发送的顺序不一致，同样的数据包被反复接收到，数据包丢失等各种类型的错误。对于追求传输可靠的通信协议，需要能够处理以上的各种错误。下面我们先抽象地讨论下传输可靠的通信协议（比如TCP）需要考虑哪些问题。

对于数据包丢失这样的错误，最直接的处理方法就是再传送一次，直到其被正确接收为止。这里就需要确定：

1. 如何确定接收者已经接收到数据包？
2. 如何确定接收者接收到的数据包与发送者发送的数据包是一致的？

通常的做法是接收者应答发送者，告知已经接收到数据包。当发送者接收到应答后，再发送下一数据包，这里的应答被称为`acknowledgment`，即为`ACK`。这里又存在几个问题：

1. 发送者等待接收者的应答（ACK），需要等待多长时间？
2. 如何ACK在传输过程中丢失了，应该如何处理？
3. 如果数据包被接收者接收到了，但是数据包中某些数据位发生了错误，该如何处理？

对于问题1，现在回答比较困难，待后续讨论到TCP的细节时，再回顾这个问题。

对于问题2，其实发送者是无法区分到底是数据包在传输过程中丢失了，还是应答在传输过程中丢失了。因此发送者的处理办法就是简单的再将数据包重新发送一遍。当然接收者可能接收到两个或者多个同样的数据包，因此接收者需要有能力处理这种情况。

对于问题3，一般而言，检测错误比纠正错误更加容易实现，因此可以通过在数据包中增加错误检测机制，比如CRC等。若接收者接收到包含错误的数据包，则接收者需避免发送应答ACK，这样让发送者再重新发送一遍数据包。

通过以上的讨论可以知道，在传输过程中，接收者完全是有可能重复接收到同一个数据包的，而对于接收者，是通过`sequencec number`来识别接收到的数据包是否是以前接收过的。

目前讨论的协议，虽然可靠，但是并不高效。每发送一个数据包，发送者发送完成后，都需要停下来，等待应答（ACK）。收到应答后，才发送下一个数据包，这里等待ACK的时间内，发送者什么也没做，因此这样的协议发送效率是比较低下的。特别是当数据包在通信链路中的来回时间比较长时（`the round-trip time, RTT`），效率更加低。

假设网络不会丢失数据包，也不会损坏数据包，则导致网络传输效率低下的原因，主要是我们没有让网络充分的忙起来（`more busy`）。如果我们让网络中同时传输多个数据包，则可以提升传输效率。

为了让网络忙起来，现在发送者不仅要决定何时向网络中发送数据包，还要决定发送多少个数据包，另外，发送者还需要决定如何计算等待应答的时间（计时器），还需在本地保存一份未接受到应答的数据包的副本，以备可能需要重新发送。对于接收者，需要机制能够区分哪些数据包已经接收过，哪些数据包还未接收。接收者还需要合适的数据缓冲区，以缓存“乱序”到达的数据包。另外，还需要考虑如果接收者的数据包处理速率小于发送者的发送速率，该如何处理？如果传输路径中的某些路由器，不能够按照发送者或者接收者期望的传输速率转发数据，该如何处理？

这里我们定义一个概念`window`，表示已经被发送者发送出去，但是还未接收到应答的多个数据包的集合。`window size`表示这个集合中数据包的数量。

![](figure12_1.png)

上图表示了`window`的意思。`window size`为3，数据包3已经接收到了应答ACK，因此发送者的缓冲区的数据包3副本可以被释放了。数据包7还不能发送至网络中，还需等待。若数据包4接收到了应答，则`window` 可以向右滑动一个位置，此时数据包7可以发送出去。因此这样的协议被称为`a sliding window protocol`。






