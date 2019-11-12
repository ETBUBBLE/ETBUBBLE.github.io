---
title: FWT快速沃尔什变换-简解及证明
date: 2019-09-09 20:46:45
tags:
    - FFT/NTT/FWT
categories:
    - ACM
    - 数论
mathjax: true
---
## FWT简介
FWT 用来求卷积，和`FFT`很像.
三个公式
$$
C_k=\sum_{i|j=k}A_i * B_j 
$$
$$
C_k=\sum_{i\&j=k}A_i * B_j
$$
$$
C_k=\sum_{i\ xor\ j=k}A_i * B_j
$$
三种卷积，对于三种只详解其中一种。
## 1.或(or)卷积
根据对`FTT`的理解，首先第一步转换成点值，对$C_k=\sum_{i\ +\ j=k}A_i \* B_j$来说，就有点值相同相乘$y_c=y_a \* y_b$
> `y` 表示在取某个值的时候多项式的取值。

对于上述这个卷积明显不合适，所以我们需要构造一个新的式子$FWT$满足$FWT(C)=FWT(A) \* FWT(B)$。
满足或(or)卷积的式子就是$FWT(A)[i]=\sum_{j|i=i}A[j]$,如何来的就不知道了，但是可以证明这个是对的。
就相当于$FWT(A|B)=FWT(A) \* FWT(B)$,对于公式证明，还不清楚，但是暴力打个表，用数学归纳法证明见[大佬博客](https://www.cnblogs.com/cjyyb/p/9065615.html)，倒是没有错.
$$
\sum_{i|k=k}C_i=\sum_{i|j=k}(\sum_{t|i=i}A_t*\sum_{p|j=j}B_p)
$$
$$
\sum_{i|k=k}(\sum_{o|l=i}A_o * B_l)=\sum_{i|j=k}((\sum_{t|i=i}A_t) * (\sum_{p|j=j}B_p))
$$
上式不难看出是相同的,可以简单理解下，右边拆开里面的$A_t * B_p$肯定都是在左边的子集内。

知道上述满足了，就简单了。
对于`FFT`先变成点集,然后逆变换回来，上述同`FFT`，先变成$FWT$,然后逆变换回来。那么问题来了，求$FWT$,不也是$O(n^2)$,这个时候就是FWT的核心了。
对于或卷积的$FWT$有如下式子
$$
FWT(A) =
\begin{cases}
FWT(A_0,A_1+A_0)  & [A]>1\\
A, & [A]=1
\end{cases}
$$
$A_0$为前半段，$A_1$为后半段。这个很容易证明，就假设有4项$\{a_0,a_1,a_2,a_3\}$,然后使用分治处理第一次，可以得到$\{a_0,a_0+a_1,a_2,a_2+a_3\}$，然后第二次就是$\{a_0,a_0+a_1,a_0+a_2,a_0+a_1+a_2+a_3\}$，$A$求完了，举了这个栗子应该就明白了，这个$A$的长度要是$2^k$,这个很容易解决，不足的地方补`0`就行了。逆变换就是把`+`变成`-`就好了。

## 2.和(and)卷积
这些都简单过一下$FWT$式子为$FWT(A)[i]=\sum_{j\&i=i}A[j]$
有如下式子
$$
FWT(A)=\begin{cases}(FWT(A_0+A_1),FWT(A_1)) & [A]\gt1 \\ A & [A]=1\end{cases}
$$

## 3.异或(xor)卷积
对于异或卷积有那么一点不一样，构建的式子有点差距。
$FWT$式子为$FWT(A)[i]=\sum_{i=0}^{n}=(-1)^{|(i|x)|}A[j]$
>|x| 二进制1的个数

$$
FWT(A)=\begin{cases}(FWT(A_0)+FWT(A_1),FWT(A_0)-FWT(A_1)) & n>1\\A & [A]=1\end{cases}
$$
对于这个的逆变换有点差别
$$
IFWT(A)=(\frac{IFWT(A_0)+IFWT(A_1)}{2},\frac{IFWT(A_0)-IFWT(A_1)}{2})
$$
不用变符号直接除`2`就行了。

