---
title: C++ sock编程学习笔记
date: 2019-04-19 17:19:31
tags:
    - 计算机网络
    - 笔记
    - sock 编程
categories:
    - 计算机网络
    - 笔记
---
## 主机字节序和网络字节序
CPU累加器一次能装载至少4个字节，即一个整数。
字节序分为大端字节序和小端字节序。**大端字节序**高位字节（23-31bit）储存在内存的低处，低位字节储存在高地址初。**小端字节序**恰恰相反。大部分PC电脑电脑使用小端字节序，所以又被称为主机字节序。大端字节序被称为网络字节序。***JAVA虚拟机使用的是大端字节序***
4个函数函数用来完成字节序转换。
```
// #include<netinet/in.h> //linux 下头文件
#include<winsock2.h>   //window 下头文件
unsigned long htonl(unsigned long);     //host to net 转换IP
unsigned long ntohl(unsigned long);     //net to host 转换IP
unsigned short htons(unsigned short);   //host to net 转换端口
unsigned short ntohs(unsigned short);   //net to host 转换端口
```