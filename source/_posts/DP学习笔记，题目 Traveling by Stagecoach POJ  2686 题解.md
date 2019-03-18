---
title: DP学习笔记，题目 Traveling by Stagecoach POJ  2686 题解
date: 2018-05-19 22:36:53
tags: CSDN迁移
---
  **Traveling by Stagecoach **POJ 2686，题解。

 作为一名菜鸟，说状压DP，还是有点勉强，顶多做个学习笔记。

 

 首先，什么是DP，状态转移，其实就是从已经确定的状态，到一个状态。

 状压DP，我理解的就是 用 一个数的二进制表达状态。 1，表示 有 ，0 表示无

 

 比如 4而进制表示 100 ， 说明 3号 位置表示 有 ，其它的都表示没有。

 

 题目 **Traveling by Stagecoach **POJ 2686

 开一个DP【S】[M]. S 是票的使用状况 ， M，是在哪一个城市。值就是最小花费。

 

 

 1. 把一个票都没有用的，起点 标记为0,也就是 DP[1<<n-1][a]==0.

 2. 暴力枚举 所有 可以用的票 ，(S>>i)&1 表示第 I 张票可不可以用。

 3. 再暴力枚举 当前这个S下所有的 可以到的城市，dp[S][v]!=INF，当前S下v这个城市可不可以到达。

 4. 然后再暴力所有 v， 这个城市所有的路，然后使用第 i张票。d[v][u]>=0。V 和u中间的路。

 5. 然如果用这张票，走这条路 到目的地的值小就覆盖前面的值S&~(1<<i)使用第i张票后的状态，d[v][u],从 v城市到u城市的路。

 dp[S&~(1<<i)][u]=min(dp[S&~(1<<i)][u],dp[S][v]+(double)d[v][u]*1.0/t[i])。

 

 

 AC代码：

 

 
```
 #include<iostream>
#include<algorithm>
#include<cstring>
#include<string>
#include<vector>
#include<cstdio>
#include<stdio.h>
#include<cmath>
#include<queue>
#include<map>
#include<set>
using namespace std;
typedef long long ll;
typedef unsigned long long ull;

const long long mod=1e9+7;
const int maxn=9;
const int maxm=31;
const int INF=0x7fffffff;
const int inf=0x3f3f3f3f;
const double eps=1e-8;

int n,m,a,b,p,u,v,c;

int t[maxn];
int d[maxm][maxm];

double dp[1<<maxn][maxm];


int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);
//    freopen("123.txt","r",stdin);
    while(cin>>n>>m>>p>>a>>b) {
        int k=(1<<n)-1;
        if(n==0&&m==0&&p==0&&a==0&&b==0)return 0;
        for(int i=0; i<n; i++)cin>>t[i];
        memset(d,-1,sizeof(d));
        while(p--) {
            cin>>u>>v>>c;
            d[u][v]=c;
            d[v][u]=c;
        }
        for(int i=0; i<=k; i++)fill(dp[i],dp[i]+m+1,INF);
        dp[k][a]=0;
        double res=INF;
        for(int S=k; S>=0; S--) {
            res=min(res,dp[S][b]);
            for(int i=0; i<n; i++) {
                if((S>>i)&1) {
                    for(v =1; v<=m; v++) {
                        if(dp[S][v]!=INF) {
                            for(u=1; u<=m; u++) {
                                if(d[v][u]>=0) {
                                    dp[S&~(1<<i)][u]=min(dp[S&~(1<<i)][u],dp[S][v]+(double)d[v][u]*1.0/t[i]);
                                }
                            }
                        }
                    }
                }
            }
        }
        if(res==INF) {
            printf("Impossible\n");
        } else {
            printf("%.3f\n",res);
        }
    }
    return 0;
}

```
 

 

 

   
 