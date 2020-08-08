---
title: UNIX网络编程笔记
layout: post
date:   2020-07-03 20:56:09 +0800
categories: 秋招
tags:
	- UNIX
	- 网络编程
	- 计算机网络
	- 面经
	- 秋招
---

[TOC]

# TCP连接的建立和终止

## 三路握手

（1）服务器必须准备好接受外来的连接。通常调用`socket`、`bind`、`listen`三个函数完成。称为被动打开（passive open）   

（2）客户端通过`connect`发起主动打开（active open）。这导致客户TCP发送一个SYN（同步）分节，它告诉服务器客户将在（待建立的）连接中发送数据的初始序列号。通常SYN分节不携带数据，其所在的IP数据只包含一个IP首部、一个TCP首部及可能有的TCP选项。

（3）服务器必须确认（ACK）客户的SYN，同时自己也得发送一个SYN分节，它含有服务器将在同一连接中发送的数据初始序列号。服务器在单个分节中发送SYN和对客户SYN的ACK（确认）。

（4）客户必须确认服务器的SYN。

![这里写图片描述](https://gitee.com/zyuegege/images/raw/master/imgs/20180808105159546.png)