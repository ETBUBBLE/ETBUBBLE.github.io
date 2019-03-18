---
title: ZOJ 2532 Internship(网络流求割边)
date: 2018-08-02 22:23:26
tags: CSDN迁移
---
  [ZOJ2532](http://acm.zju.edu.cn/onlinejudge/showProblem.do?problemId=1532)

 Internship

 
--------
Time Limit: 5 Seconds Memory Limit: 32768 KB

 
--------
CIA headquarter collects data from across the country through its classified network. They have been using optical fibres long before it's been deployed on any civilian projects. However they are still under a lot pressure recently because the data are growing rapidly. As a result they are considering upgrading the network with new technologies that provide a few times wider bandwidth. In the experiemental stage, they would like to upgrade one segment of their original network in order to see how it performs. And as a CIA intern it's your responsibility to investigate which segment could actually help increase the total bandwidth the headquarter receives, suppose that all the cities have infinite data to send and the routing algorithm is optimized. As they have prepared the data for you in a few minutes, you are told that they need the result immediately. Well, practically immediately.

 **Input**

 Input contains multiple test cases. First line of each test case contains three integers n, m and l, they represent the number of cities, the number of relay stations and the number of segments. Cities will be referred to as integers from 1 to n, while relay stations use integers from n+1 to n+m. You can saves assume that n + m <= 100, l <= 1000 (all of them are positive). The headquarter is identified by the integer 0.

 The next l lines hold a segment on each line in the form of a b c, where a is the source node and b is the target node, while c is its bandwidth. They are all integers where a and b are valid identifiers (from 0 to n+m). c is positive. For some reason the data links are all directional.

 The input is terminated by a test case with n = 0. You can safely assume that your calculation can be housed within 32-bit integers.

 **Output**

 For each test print the segment id's that meets the criteria. The result is printed in a single line and sorted in ascending order, with a single space as the separator. If none of the segment meets the criteria, just print an empty line. The segment id is 1 based not 0 based.

 **Sample Input**

 
```
 2 1 3 1 3 2 3 0 1 2 0 1 2 1 3 1 3 1 2 3 1 3 0 2 0 0 0 
```
 **Sample Output**

 
```
 2 3 <hey here is an invisible empty line>
```
 题意：就是给你几个点 然后全部要汇到 终点 0 问哪几条边流量上升可以直接增大流量。

 题解： 首先用网络流跑一边，然后一个是起点的残余网络，一个是终点的残余网络，如果有一条变能从起点残余网络跑到终点的残余网络肯定 就是因为他限制了流量，所以这题就是找能连接两个残余网络的的边。

 起点残余网络，就顺着你连的边流一次就行了。

 终点残余网络是逆着流一遍，是用反向边跑，这一点需要注意下，因为这个残余网络是从别的点汇聚到终点，不是从重点流出去。把所有能连接两边点的且边的流量是0的边加入答案就行，最后排个序。

 怕理解不了插个图

 ![](https://img-blog.csdn.net/20180802222203919?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

 AC代码

 
```
 #include<iostream>
#include<algorithm>
#include<cstring>
#include<string>
#include<vector>
#include<cstdio>
#include<cmath>
#include<queue>
#include<map>
#include<set>
#include<stack>
#define mem(a,b) memset(a,b,sizeof(a))
using namespace std;
typedef long long ll;
typedef unsigned long long ull;
typedef pair<int,int> P;
const long long mod=1e9+7;
const int maxn=400+25;
const int maxm=1e5+25;
const int INF=0x7fffffff;
const int inf=0x3f3f3f3f;
const double eps=1e-8;

int n,m,l,tot;
struct edge {
    int num,to,cap,rev;
};
vector <edge> G[maxn];
int level[maxn];
int iter[maxn];

void init(int _n) {
    for(int i=0; i<=_n; i++) {
        G[i].clear();
    }
}

void bfs(int s) {
    memset(level,-1,sizeof(level));
    queue<int> que;
    level[s]=0;
    que.push(s);
    while(!que.empty()) {
        int v= que.front();
        que.pop();
        for(int i=0; i<G[v].size(); i++) {
            edge & e=G[v][i];
            if(e.cap>0&&level[e.to]<0) {
                level[e.to]=level[v] + 1;
                que.push(e.to);
            }
        }
    }
}

void add(int from,int to,int cap) {
    edge eg;
    eg.to=to;
    eg.num=tot;
    eg.cap=cap;
    eg.rev=G[to].size();
    G[from].push_back(eg);
    eg.num=2*n+m+1+l+tot++;
    eg.to=from;
    eg.cap=0;
    eg.rev=G[from].size()-1;
    G[to].push_back(eg);
}

int dfs(int v,int t,int f) {
    if(v == t)return f;
    for(int &i = iter[v]; i < G[v].size(); i++) {
        edge &e=G[v][i];
        if(e.cap>0 && level[v]<level[e.to]) {
            int d=dfs(e.to,t,min(f,e.cap));
            if(d>0) {
                e.cap-=d;
                G[e.to][e.rev].cap+=d;
                return d;
            }
        }
    }
    return 0;
}
int maxflow(int s,int t) {
    int flow=0;
    for(;;) {
        bfs(s);
        if(level[t]<0)return flow;
        memset(iter,0,sizeof(iter));
        int f;
        while((f = dfs(s,t,inf))>0) {
            flow +=f;
        }
    }
}

int dis[maxn],dis2[maxn];
void dfs1(int x) {
    dis[x]=1;
    for(int i=0; i<G[x].size(); i++) {
        if(dis[G[x][i].to]==-1&&G[x][i].cap!=0) {
            dfs1(G[x][i].to);
        }
    }
}
void dfs2(int x) {
    dis2[x]=1;
    for(int i=0; i<G[x].size(); i++) {
        if(dis2[G[x][i].to]==-1&&G[G[x][i].to][G[x][i].rev].cap!=0) {
            dfs2(G[x][i].to);
        }
    }
}
int main() {
    while(~scanf("%d%d%d",&n,&m,&l)&&(m+n+l)) {
        init(n+m+1);
        tot=1;
        for(int i=0; i<l; i++) {
            int a,b,c;
            scanf("%d%d%d",&a,&b,&c);
            add(a,b,c);
        }
        for(int i=1; i<=n; i++) {
            add(n+m+1,i,inf);
        }
        maxflow(n+m+1,0);
        mem(dis,-1);
        mem(dis2,-1);
        dfs1(n+m+1);
        dfs2(0);
        vector<int> v;
        v.clear();
        for(int i=1; i<=n+m; i++) {
            if(dis[i]==1) {
                for(int j=0; j<G[i].size(); j++) {
                    if(G[i][j].num<=l&&dis2[G[i][j].to]==1&&G[i][j].cap==0) {
                        v.push_back(G[i][j].num);
                    }
                }
            }
        }
        if(v.size()==0) {
            puts("");
        } else {
            sort(v.begin(),v.end());
            for(int i=0; i<v.size(); i++) {
                printf("%d%c",v[i],i+1==v.size()?'\n':' ');
            }
        }
    }
    return 0;
}

```
 

   
 