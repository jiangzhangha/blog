---
title: Go语言学习笔记之程序结构
date: 2020-09-06 15:07:00
categories: Go语言
index_img: /img/articles/go1.jpg
tags:
	- Go语言
---

本文是经典书籍《The Go Programming Language》的学习笔记，主要为第二章程序结构相关的学习。

<!-- more -->

## 命名

命名规则：以一个字母或者下划线开头，后面可以跟随任意数量的数字、字母、或者下划线。

在Go语言中，大写字母与小写字母是不同的，因此字符`hello`与`Hello`在Go语言中是不同的字符表达。

名字首字母的大小决定了名字的可见性，如果一个名字的首字母是大写，且该名字是在函数外部定义的包级别的名字，则该名字是导出的，即可以被外部的包访问。

包的名字一般总是使用小写字母。

## 关键字

Go语言中的关键字目前共有25个，这些关键字不能用于自定义的名称。25个关键字主要包括：

`break case chan const continue default defer else fallthrough for func go goto if import interface map package range return select struct switch type var`

另外，Go语言中还有一些预定义的名字，主要对应内建的常量，数据类型，函数等。这些预定义的名字并不是关键字，程序员可以在程序中重新使用这些名字，但是要注意防止引起语义混乱。预定义的名字主要包括：

内建常量：

`true false itoa nil`

内建类型：

`int int8 int16 int32 int64`

`uint uint8 uint16 uint32 uint64 uintptr`

`float32 float64 complex64 complex128`

`bool byte rune string error`

内建函数：

`make len cap new append copy close delete complex real imag panic recover`

## 声明

Go语言主要有四种类型的声明语句：`var const type func`

## 简短变量声明

在函数内部，简短变量声明可以用于声明和初始化局部变量。格式为：

`变量名称 := 表达式`

变量的类型是根据表达式的类型来自动推导的。简短变量声明一般被广泛应用于大部分的局部变量的声明与初始化。

而`var`类型的变量声明往往是用于需要指定变量类型的地方。

## new函数

调用内建的new函数也可以创建变量，但是该变量是匿名变量，表达式`new(T)`将创建一个T类型的匿名变量，new函数初始化变量为T类型的零值，然后返回匿名变量的地址，即返回`*T`指针类型。






