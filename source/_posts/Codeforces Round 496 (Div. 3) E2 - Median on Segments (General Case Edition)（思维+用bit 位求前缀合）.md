---
title: Codeforces Round 496 (Div. 3) E2 - Median on Segments (General Case Edition)（思维+用bit 位求前缀合）
date: 2018-07-16 18:44:30
tags: CSDN迁移
---
  这题看了别人的博客，看的我一脸懵逼。

 思路：很巧秒的转换，我们把<= m 数记为-1, >m的数 记为1， 求其前缀和, 我们将问题转变成求以> m 的数作为中位数的区间个数，

 答案就变为ans(m-1) - ans(m )，我们可以用上面求得的前缀用bit就能求出答案。

 我特么还不知道是这样写的么，我是不知道怎么用前缀。然后纠结了半天，是咱的基础不好。

 所以重点是怎么用bit位来处理前缀和呢？

 只可意会不可言传

 
```
 5 4
1 4 5 60 4
```
 首先传个 当 k =4 时

 就是 -1 -2 -1 0 -1

 首先 知道 前缀和为负数的中卫肯定是<=4为正数的一定是>4

 结点图如下，至于8以上的结点就不画了，画了也没用前缀合最大值不会超过8；

 ![](https://img-blog.csdn.net/20180716175605831?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

 首先 0 +n+1=5 （初始值都为-1） 和这个以后的结点值 ++；

 也就是 6 8结点 都加1；

 下一个值 -1

 把所有 -1+n=4 一下的结点 的值都加起来。

 然后 把 小于 -1+n 的结点和都加一，也就是 4 8都加1；

 后面的都是同样的道理

 

 
```
 #include<bits/stdc++.h>
using namespace std;
const int maxn=2e5+5;
const int N=2*maxn;
int n,m,a[maxn];
int v[N];

void jl(int x) {
    for(int i=x; i<N; i+=i&-i) {
    //这个是来保留的前缀和，假如 当前位置的前缀合 是 -1 ，因为可能出现负数的情况所以加上 n
    // 然后比 -1 + n 向上跳转的结点 都加上1
        v[i]++;
    }
}

long long s(int x) {
    long long sum=0;
    for(int i=x; i>0; i-=i&-i) {
        sum+=v[i];
    // 计算前缀和，从上往下加，这样会把小于当前结点的值都加起来。
    }
    return sum;
}

long long cal(int k){
    memset(v,0,sizeof(v));
    jl(n+1);
    int dp=0;
    long long sum=0;
    for(int i=0;i<n;i++){
        if(a[i]<=k)dp-=1;
        else dp+=1;
        sum+=s(dp+n);
        jl(dp+n+1);
    }
    return sum;
}

int main() {
    scanf("%d%d",&n,&m);
    for(int i=0; i<n; i++)scanf("%d",&a[i]);
    cout<<cal(m-1)-cal(m)<<endl;
    return 0;
}

```
 

 

   
 