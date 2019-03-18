---
title: bit 位，树状数组学习笔记。
date: 2018-04-25 15:59:39
tags: CSDN迁移
---
  给一个初始值全为0的数列a1,a2,...,an.

给定 i，求a1+a2+..+ai.

给定i,x 执行ai+x;



![](https://img-blog.csdn.net/20180425153924955?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)



图不好看见谅：

如图所示，1节点维护的是a1本身的和

2节点维护的是 a1到a2 的和

3节点维护的是a3的和

4节点维护的是a1到a4 的和

为啥会有些节点维护的值的个数不同呢？

很简单 ，就是看最后一个1的位置，2：二进制0010 最后一个1是第2个位置所以维护2的2-1次方个值。4：0100维护2的3-1次方个值。

  
![](https://img-blog.csdn.net/20180425154248626?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)





加法，把有维护自己的值都加上X就行了

加法：例子在a3上+X就是在3节点上+X，4节点+X，8节点+X。看一眼上图，就知道，就是按箭头一个个向上节点转移。

怎么实现这种转移呢？

3的二进制是0011.4是 0100，8是1000.

就是把加上最后一个1所在位置的值，0011 +0001（最后一个位置是最后一个）=0100；

0100+0100（最后一个1位置是第3个）=1000；

以此类推，在a5也是一样,0101+0001=0110(6);

0110+0010=1000(8);

怎么实现，就是i+=i&-i;

i&-i可以把自己最后一个1的位置算出来，怎么来的就自己去百度吧。





求和：比如 前5个的和，就是（1到4）+5 也就是4节点加5节点的和。

前7个的和就是7+（5到6）+（1到4）的和，也就是4节点加6节点加7节点的和。



至于怎么实现呢如果细心的话不难发现，其实就是从最后一个1慢慢一个个变成0的节点全加上 如 7的二进制是0111

前7个的和就是 0111+ 0110+0100

前 5（0101）个的和 0101 +0100

按位运算就是 i-=i&-i.

代码非常简单。

复杂度 O（log N）;

#include<bits/stdc++.h>  
using namespace std;  
const int maxn=1<<15;  
int bit[maxn+1],n;  
int sum(int i)  
{  
 int s=0;  
 while(i>0)  
 {  
 s +=bit[i];  
 i-=i&-i;  
 }  
 return s;  
}  
void add(int i,int x)  
{  
 while(i<=n)  
 {  
 bit[i]+=x;  
 i+=i&-i;  
 }  
}  




   
 