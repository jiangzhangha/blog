---
title: 《TCP/IP详解》协议卷读书笔记之 网络地址
date: 2020-08-08 14:01:00
categories: 网络
index_img: /img/articles/tcp_ip2.jpg
tags:
	- 网络
	- TCP
	- IP
---

本文是经典著作《TCP/IP详解》协议卷（卷一）的读书笔记，对第二章网络地址部分进行总结与梳理。

<!-- more -->

## IP地址的表示方式

IPv4的IP地址与IPv6的IP地址是不相同的。IPv4的IP地址只有32位，而IPv6的IP地址有128位，因此两者的表示方式也不相同。

对于IPv4的IP地址表示方式，接触网络的用户应该对其不陌生，因此这里对其不再赘述。

而很多用户对IPv6的IP地址相对比较陌生一些。一般通过使用冒号（:）分隔一系列的十六进制数字的方式来表示IPv6的IP地址。比如：`5f05:2000:80ad:5800:0058:0800:2023:1d71`

由于IPv6的IP地址共用128位，如果将每一位都写出来，数字串比较长，因此形成了一些简化写法：

- 前导的零可以省略写，比如之前的地址可以写成`5f05:2000:80ad:5800:58:800:2023:1d71`
- 全为零的数字块可以省略写。比如，地址为`0:0:0:0:0:0:0:1`，可以简写为`::1`。地址为`2001:0db8:0:0:0:0:0:2`，可以简写为`2001:db8::2`，为了避免混淆，`::`符号只能在地址表示中使用一次
- 可以在IPv6的IP地址中嵌入IPv4的IP地址，在IPv4的IP地址之前为固定的十六进制值`ffff`，比如，IPv6的IP地址`::ffff:10.0.0.1`表示IPv4的IP地址`10.0.0.1`，这样的表达方式被称为`IPv4-mapped IPv6 address`
- IPv6的IP地址的低32位可以使用点分四组表示法，因此，IPv6的IP地址`::0102:f001`可以写成`::1.2.240.1`，这样的表达方式被称为`IPv4-compatible IPv6 address`

另外，IPv6的IP地址使用冒号分隔，而URL中，地址段与端口号段也是使用冒号分隔，若URL使用IPv6的IP地址与端口号的方式来表达，容易引起混淆，可以使用`[]`符号将IP地址段包含起来，避免歧义，如：

`http://[2001:0db8:85a3:08d3:1319:8a2e:0370:7344]:443/`

## IP地址结构

一般将IP地址划分为不同的组，各个组具有不同的用途。IP地址空间划分为单播地址、组播地址、广播地址与任播地址。

### 地址分类

Internet地址在初始发展阶段时，将IP地址划分为两个段，分别为network段与host段，network段表示该IP地址所在的网段，而host段用来识别该网段内特定的主机。

在实际使用中，逐渐认识到在不同的网络中，有不同的主机数量，因此将IPv4的IP地址划分为5种不同的类型。每一种类型都是对网络中的网段数量与主机数量的权衡。具体划分方式如下：

![](figure2_1.png)

按照这种类型划分后，IPv4的IP地址空间数量为：

![](table2_3.png)

随着网络的发展，这样的网络地址分类方法逐渐遇到问题，当新的网段被添加到Internet，如何为其分配合适的A类型、B类或者C类地址将难以抉择，因为网段数量与主机数量与A、B、C类型难以匹配。A、B类型网络地址浪费太多的主机数量，而C类型网络地址有不能提供足够多的主机数量。

### 子网寻址

为了解决上节提到的问题，人们想到的解决方案是：将A、B、C类型的IP地址的host段，在细划分为subnet段与host段，而具体如何划分，可以将其留给本地网络管理员决定。在该网络之外的Internet网络，依然将其看成是一个A、B或者C类型的IP地址。

比如，对于一个B类型的IP地址，network段依然保持16位不变，而原16位的host段，本地网络管理员划分为8位的subnet段，以及8位的host段，这样，可以将网络划分为256个子网，每个子网可以有254个主机（子网的第一个地址与最后一个地址不能被主机使用）。通过子网的划分，主机数量可以不拘泥于特定的网络地址类型，本地网络管理员可以根据实际的网络情况与主机数量，划分合适的子网与主机数量，大大提高了IP地址的使用率。

[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-19FUStat-1596875977787)(figure2_2.png)]

对于该网络之外的其他Internet，是看不到subnet的概念的，依然将后16位看成是B类型IP地址的host段。但是对于该网络内的路由器而言，其需要知道subnet段与host段如何划分，不然内部的路由器将无法知道数据包的传输路径。

### 子网掩码

子网掩码是用来区分一个IP地址的network/subnet段与host段的分界处。子网掩码的长度与IP地址的长度是相同的。子网掩码一般是由一系列的1，紧跟着一系列的0组成，子网掩码中1所在的位所对应的IP地址的位，即是network/subnet段，而子网掩码中0所在的位所对应的IP地址的位，即是host段。

将子网掩码与IP地址按照位进行“AND”操作，获得的即是network/subnet段，网络的内部路由器即可使用该network/subnet段进行数据的转发。举个例子：

![](figure2_4.png)

需要注意，子网掩码只是本地局域网所看到的概念，对于该局域网之外的网络，是不关心subnet的概念的，只需要通过IP地址的头几位数据确定为A或者B或者C类型的IP地址，进而确定network段，即可进行数据的转发。

### 广播地址

在每一个IPv4的子网中，有一个特殊的地址将被保留为子网广播地址（`subnet broadcast address`）。子网广播地址，是将地址的network/subnet段设置为对应网段的值，将host段设置为全1，即组成了该子网的广播地址。

另外特殊的地址`255.255.255.255`被保留为`local net broadcast`，该地址上的数据不会被路由器转发。

## CIDR与聚合

### 问题

在20世纪90年代初期，随着Internet规模的增长，Internet面临更严重的规模问题，书中列出了三个比较重要的问题：

1. 到1994年，一半以上的B类地址将被分配掉，在1995年，B类地址将全部被使用掉；
2. 32位的IPv4地址被认为不足以应付21世纪的网络规模；
3. 全球性路由表的数目一直在增长，越来越多的路由条目的出现，严重影响了路由性能。

IETF中的一个小组ROAD（`routing and addressing`）开始关注这三个问题。小组认为，通过IPv6解决第二个问题，取消IP地址的分类方法，使用CIDR（`classless inter-domain routing`）解决第一个问题，使用分层路由的思想，采用聚合（`aggregation`）的方法解决第三个问题。

### CIDR

为了解决IP地址不够用的问题，ROAD小组提出了取消对IP地址的A、B、C的分类方式，转而使用一个`prefix`的方法，来确定IP地址的范围。

prefix是一个数字，对于IPv4，其取值范围为0~32、对于IPv6，其取值范围为0~128，对于值为n的prefix，表示其对应的IP地址的前n位数值为固定值，余下的位的数值可灵活变动，因此可将IP地址分配为不同的范围。

这样的分类方法也兼容A、B与C类的分类方法，比如A类IP地址，其prefix为8，B类IP地址，其prefix为16，而C类IP地址，其prefix为24。

一些例子为：

![](table2_6.png)

### 聚合

在全球性路由表中路由条目的增多，验证影响了路由效率。

书中做了一个形象的比喻：要比驾驶一辆车去目的地，在不知道目的地地址的情况下，根据各个路段的路标指示牌，来确定下一段路的方向，这样根据一段一段的指示，最终到达目的地。但是如果在整个系统中，可以到达的目的地增多的情况下，如果简单的只是在每个路段增加指示牌，则在路途中，每到一个路口，都需要从很多的指示牌中选出一个正确的指示牌，来确认方向。这样就比较的麻烦，效率也会下降。为了减少指示牌，可以将目的地进行分类，指示牌只是指出某种类型的目的地的方向，这样，指示牌的数量将会减少很多。这也是分层路由与聚合的思想。

Kleinrock 与 Kamoun 通过研究发现，如果网络的拓扑结构调整为树形结构，网络地址按照树形拓扑结构进行分配，则可以在保持最短路径有效的情况下，大大减少路由条目。

![](figure2_8.png)

 根据这种路由分层的思想，采用一种被称为`route aggregation`的方法，减少路由条目数。即可以将多个IP地址的相邻前缀，聚合为一个短的前缀，即将相邻的IP地址进行合并，进而达到减少路由条目的目的。

![](figure2_9.png)

## 特殊用途的地址

在IP地址分配中，存在一些特殊用途的地址，对于IPv4的地址，存在以下的特殊用途地址：

![](table2_7.png)

对于IPv6的地址，存在以下的特殊用途的地址：

![](table2_8.png)





