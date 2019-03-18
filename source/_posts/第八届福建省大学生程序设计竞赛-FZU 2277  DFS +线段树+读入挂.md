---
title: 第八届福建省大学生程序设计竞赛-FZU 2277  DFS +线段树+读入挂
date: 2018-08-20 20:29:30
tags: CSDN迁移
---
  [FZU 2277](http://acm.fzu.edu.cn/problem.php?pid=2277)

 ![](http://acm.fzu.edu.cn/image/problem.gif)** Problem 2277 Change**

 
### Accept: 245 Submit: 1186  
 Time Limit: 2000 mSec Memory Limit : 262144 KB

 
## ![](http://acm.fzu.edu.cn/image/prodesc.gif) Problem Description

 There is a rooted tree with n nodes, number from 1-n. Root’s number is 1.Each node has a value ai.

 Initially all the node’s value is 0.

 We have q operations. There are two kinds of operations.

 1 v x k : a[v]+=x , a[v’]+=x-k (v’ is child of v) , a[v’’]+=x-2*k (v’’ is child of v’) and so on.

 2 v : Output a[v] mod 1000000007(10^9 + 7).

 
## ![](http://acm.fzu.edu.cn/image/prodesc.gif) Input

 First line contains an integer T (1 ≤ T ≤ 3), represents there are T test cases.

 In each test case:

 The first line contains a number n.

 The second line contains n-1 number, p2,p3,…,pn . pi is the father of i.

 The third line contains a number q.

 Next q lines, each line contains an operation. (“1 v x k” or “2 v”)

 1 ≤ n ≤ 3*10^5

 1 ≤ pi < i

 1 ≤ q ≤ 3*10^5

 1 ≤ v ≤ n; 0 ≤ x < 10^9 + 7; 0 ≤ k < 10^9 + 7

 
## ![](http://acm.fzu.edu.cn/image/prodesc.gif) Output

 For each operation 2, outputs the answer.

 
## ![](http://acm.fzu.edu.cn/image/prodesc.gif) Sample Input

 1 3 1 1 3 1 1 2 1 2 1 2 2

 
## ![](http://acm.fzu.edu.cn/image/prodesc.gif) Sample Output

 2 1

 
## ![](http://acm.fzu.edu.cn/image/prodesc.gif) Source

 

 题意：给你一棵树 有两种操作：查询节点的值，和将所有树节点及以下下所有的节点 + x - (子节点深度-当前深度)*k 的值

 题解：首先肯定是DFS建序，然后根据dfs 序建一颗线段树，这题更新操作是更新一个区间，查询是单点。

 这题只要用一个dep数组保存每个节点所包含区间里面的最小深度，然后向下更新的时候每次把，x，和k，更新下去；

 更新的时候直接把，(子节点深度-当前深度)*k的值更新到 ，x,里面。

 查询直接返回单点的x.

 这题主要是卡取模和读入。。。读入特么就快超时了，最让我无语的还是超时给我返回一个WA加个读入挂就过了。。。。

 
```
 #include<iostream>
#include<algorithm>
#include<cstring>
#include<string>
#include<vector>
#include<cstdio>
#include<cstdlib>
#include<cmath>
#include<queue>
#include<map>
#include<set>
#include<stack>

using namespace std;
typedef long long ll;
typedef ll LL;
typedef unsigned long long ull;
typedef pair<int,int> P;

#define bug printf("*********\n");
#define debug(x) cout<<"["<<x<<"]" <<endl;
#define mid (l+r)/2
#define chl 2*k+1
#define chr 2*k+2
#define lson l,mid,chl
#define rson mid,r,chr
#define pb push_back
#define mem(a,b) memset(a,b,sizeof(a));

const long long mod=1e9+7;
const int maxn=3e5+5;
const int INF=0x7fffffff;
const int inf=0x3f3f3f3f;
const double eps=1e-8;
int e[maxn],s[maxn],tdep[maxn];
int cnt=0;

struct node {
    ll x,k;
} lazy[maxn<<4];
int dep[maxn<<4];
struct edge {
    int to,next;
} eg[maxn];
int head[maxn],tot;
void add(int u,int v) {
    eg[tot].to=v;
    eg[tot].next=head[u];
    head[u]=tot++;
}
void init() {
    tot=0;
    cnt=0;
    mem(head,-1);
}
int dfs(int r,int dp) {
    cnt++;
    s[r]=cnt;
    tdep[cnt]=dp;
    for(int i=head[r]; i!=-1; i=eg[i].next) {
        int to=eg[i].to;
        dfs(to,dp+1);
    }
    e[r]=cnt;
}

void build(int l,int r,int k) {
    if(r-l==1) {
        dep[k]=tdep[r];
        lazy[k].k=0;
        lazy[k].x=0;
    } else {
        build(lson);
        build(rson);
        dep[k]=min(dep[chl],dep[chr]);
        lazy[k].k=0;
        lazy[k].x=0;
    }
}
void pushdown(int l,int r,int k) {
    if(lazy[k].k==0&&lazy[k].x==0) return ;
    lazy[chl].k+=lazy[k].k;
    lazy[chl].k%=mod;

    lazy[chr].k+=lazy[k].k;
    lazy[chr].k%=mod;

    lazy[chl].x+=(lazy[k].x-lazy[k].k%mod*(dep[chl]-dep[k])+mod)%mod;
    lazy[chl].x=(lazy[chl].x+mod)%mod;
    lazy[chr].x+=(lazy[k].x-lazy[k].k%mod*(dep[chr]-dep[k])+mod)%mod;
    lazy[chr].x=(lazy[chr].x+mod)%mod;
    lazy[k].x=0;
    lazy[k].k=0;
}
void update(int a,int b,int l,int r,int k,ll x,ll y,ll dp) {
    if(b<=l||a>=r) {
        return ;
    } else if(a<=l&&r<=b) {
        lazy[k].x+=(x-y*(dep[k]-dp)%mod+mod)%mod;
        lazy[k].x%=mod;
        lazy[k].k+=y;
        lazy[k].k%=mod;
        return ;
    } else {
        pushdown(l,r,k);
        update(a,b,lson,x,y,dp);
        update(a,b,rson,x,y,dp);
    }
}
ll res=0;
void query(int a,int b,int l,int r,int k) {
    if(b<=l||a>=r) {
        return ;
    } else if(a<=l&&r<=b) {
        res=lazy[k].x%mod;
        return ;
    } else {
        pushdown(l,r,k);
        query(a,b,lson);
        query(a,b,rson);
    }
}

void read(LL &sum) {
    sum=0;
    char ch=getchar();
    while(!(ch>='0'&&ch<='9'))ch=getchar();
    while(ch>='0'&&ch<='9')sum=sum*10+ch-48,ch=getchar();
}
void read(int &sum) {
    sum=0;
    char ch=getchar();
    while(!(ch>='0'&&ch<='9'))ch=getchar();
    while(ch>='0'&&ch<='9')sum=sum*10+ch-48,ch=getchar();
}
int main() {
    int n,t;
    read(t);
    while(t--) {
        init();
        scanf("%d",&n);
        for(int i=2; i<=n; i++) {
            int x;
            read(x);
            add(x,i);
        }
        dfs(1,1);
        build(0,n,0);
        int q;
        read(q);
        while(q--) {
            int op;
            read(op);
            ll a,b,c;
            if(op==1) {
                read(a);
                read(b);
                read(c);
                update(s[a]-1,e[a],0,n,0,b,c,tdep[s[a]]);
            } else {
                scan_d<LL>(a);
                query(s[a]-1,s[a],0,n,0);
                printf("%lld\n",(res+mod)%mod);
            }
        }
    }
    return 0;
}

```
 

   
 