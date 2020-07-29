---
title: Linux man page学习之intro
date: 2020-07-27 18:13:00
categories: Linux知识
index_img: /img/articles/intro.jpg
tags:
	- Linux
	- intro
---

Linux man page是获取Linux系统下相关命令、系统调用等官方解释的手册，本文对man手册进行简单介绍。

<!-- more -->

在学习Linux命令期间， 很多新手现在可能会想：“Linux系统中有那么多命令，我怎么知道某个命令是干嘛用的？在日常工作中遇到了一个不熟悉的Linux命令，我又怎样才能知道它有哪些可用参数呢？”，关于这一点Linux下的 man 命令就是为此而准备的， man 命令是Linux下的帮助指令，通过 man 指令可以查看Linux中的指令帮助、配置文件帮助和编程帮助等信息。

在使用man手册之前，需要对man手册的整体有一个大概的了解，才能更好的使用man手册高效地查找所需的知识点。man手册中也对man手册本身有一个简单的介绍（即intro部分），本文对man手册的intro部分进行简介。

## 分类

通过man一下man命令，就可以获取man手册的大概简介内容，以及man命令的使用方式：

~~~shell
[jamza@jamza-vm ~]$ man man
~~~

在手册中，对man手册的分类进行了介绍：

>  The table below shows the section numbers of the manual followed by the types of pages they contain.
>  1   Executable programs or shell commands
>  2   System calls (functions provided by the kernel)
>  3   Library calls (functions within program libraries)
>  4   Special files (usually found in /dev)
>  5   File formats and conventions eg /etc/passwd
>  6   Games
>  7   Miscellaneous (including macro packages and conventions), e.g. man(7), groff(7)
>  8   System administration commands (usually only for root)
>  9   Kernel routines [Non standard]

从以上内容可以看出，man手册分为以下的9大类：

- 可执行程序或者shell命令
- 系统调用
- 库调用
- 特殊文件
- 文件格式与约定
- 游戏
- 杂项（包括宏与惯例）
- 系统管理命令（root用户使用）
- 内核例程（非标准）

对此，我们比较关心，且需要重点学习的是第1大类与第2大类的内容。

### Executable programs or shell commands

通过以下的命令，可以查看man手册的“可执行程序或者shell命令”类的简介：

~~~shell
[jamza@jamza-vm ~]$ man 1 intro
~~~

> Section  1  of  the manual describes user commands and tools, for example, file manipulation tools, shells, compilers, web browsers, file and image viewers and editors, and so on.

### System calls

通过以下的命令，可以查看man手册的“系统调用”类的简介：

~~~shell
[jamza@jamza-vm ~]$ man 2 intro
~~~

> Section  2  of  the manual describes the Linux system calls.  A system call is an entry point into the Linux kernel.  Usually, system calls are not invoked directly: instead, most system calls have corresponding  C  library  wrapper  functions which  perform the steps required (e.g., trapping to kernel mode) in order to invoke the system call.  Thus, making a system call looks the same as invoking a normal library function.
> In many cases, the C library wrapper function does nothing more than:
>
>    *  copying arguments and the unique system call number to the registers where the kernel expects them;
>    *  trapping to kernel mode, at which point the kernel does the real work of the system call;
>    *  setting errno if the system call returns an error number when the kernel returns the CPU to user mode.
>
> However, in a few cases, a wrapper function may do rather more than this, for example, performing  some  preprocessing  of the  arguments before trapping to kernel mode, or postprocessing of values returned by the system call.  Where this is the case, the manual pages in Section 2 generally try to note the details of both the (usually GNU) C  library  API  interface and  the  raw system call.  Most commonly, the main DESCRIPTION will focus on the C library interface, and differences for the system call are covered in the NOTES section.
>
> For a list of the Linux system calls, see syscalls(2).

### Library calls

查看man手册的“库调用”类的简介：

> Section  3  of the manual describes all library functions excluding the library functions (system call wrappers) described in Section 2, which implement system calls.
>
> Many of the functions described in the section are part of the Standard C Library (libc).   Some  functions  are  part  of other  libraries  (e.g., the math library, libm, or the real-time library, librt) in which case the manual page will indicate the linker option needed to link against the required library (e.g., -lm and -lrt, respectively,  for  the  aforementioned libraries).
>
> In  some  cases, the programmer must define a feature test macro in order to obtain the declaration of a function from the header file specified in the man page SYNOPSIS section.  (Where required, these feature test macros must be defined before including  any header files.)  In such cases, the required macro is described in the man page.  For further information on feature test macros, see feature_test_macros(7).



## 页面内容结构

对于每个man手册的页面内容，存在以下的主要内容结构：

- NAME：命令的名称与用途
- SYNOPSIS：命令的语法
- DESCRIPTION：命令作用的详细描述，包括其选项，参数等
- OPTIONS：选项与参数
- AUTHOR：作者
- COPYRIGHT：版权
- SEE ALSO：其他参考
- BUGS：名称存在的bugs

## 在线版

man手册的在线版本，可以在Linux的官方网站上找到：[The Linux *man-pages* project](https://www.kernel.org/doc/man-pages/)