---
title: Linux man page学习之lseek
date: 2020-07-29 13:44:00
categories: Linux知识
index_img: /img/articles/lseek.jpg
tags:
	- Linux
	- lseek
---

本文简介了类UNIX系统下（主要为Linux）的lseek相关函数的内容。

<!-- more -->

# 函数原型

~~~c
#include <sys/types.h>
#include <unistd.h>

off_t lseek(int fd, off_t offset, int whence);
~~~

# 简介

lseek()系统调用根据输入的参数whence与offset、设置并返回打开文件描述符fd对应文件的文件偏移量。

whence的取值范围为：

- SEEK_SET：文件的偏移量被设置为参数offset的值
- SEEK_CUR：文件的偏移量被设置为（当前偏移量 + offset）
- SEEK_END：文件的偏移量被设置为（文件长度 + offset）
- SEEK_DATA：Linux 3.1以后版本支持，调整文件的偏移量至大于或等于offset，且包含有效数据（非hole）的偏移处，如果offset偏移处即为有效数据，则文件偏移量被设置为offset
- SEEK_HOLE：Linux 3.1以后版本支持，调整文件的偏移量至大于或等于offset，且为hole的偏移处。如果offset指向hole的中间，则文件偏移量被设置为offset。如果文件中没有hole，则文件偏移量被设置为文件的末尾。

lseek()系统调用允许将文件偏移量设置为超出文件末尾的位置，但是这样不会改变文件的大小。如果在超出文件末尾的位置写入数据，则在此写入数据位置与文件原末尾位置直接将出现hole（空洞）。对于hole段执行read操作，则返回NULL字节（'\0'）。

若whence设置为SEEK_DATA或者SEEK_HOLE，而offset指向超出文件的末尾，则lseek()系统调用将失败。

若需要使用SEEK_DATA或者SEEK_HOLE选项，则宏 _GNU_SOURCE 需被定义。

支持SEEK_DATA或者SEEK_HOLE选项的文件系统包括：Btrfs、OCFS、XFS、ext4、tmpfs、NFS、FUSE

# 返回值

若成功，则返回从文件开头至偏移处的字节数，即偏移量

若失败，则返回 (off_t)-1，且设置errno

# 其他

数据类型 off_t 是有符号的整型数据。

标准并没有明确规定哪些设备需支持lseek，因此有些设备可能不支持lseek操作。