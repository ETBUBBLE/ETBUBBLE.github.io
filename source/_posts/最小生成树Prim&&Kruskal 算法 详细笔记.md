---
title: 最小生成树Prim&&Kruskal 算法 详细笔记
date: 2018-06-06 18:18:43
tags: CSDN迁移
---
  POJ 1258

**Agri-Net**

[http://poj.org/problem?id=1258](http://poj.org/problem?id=1258)；



两种算法 Prim Kruskal.



先说Prim



初始化 权值，随便一个顶点做起点，为0 其它的为最大值。



1. 找到权值最小的顶点，且没有加入集合。

2. 把顶点权值加到结果，把定点加入集合。

3. 暴力枚举 顶点连接的所有的边，更新所有能够连接上顶点的权值。

4. 重复前3步，直到所有点全部加入集合。



![](https://img-blog.csdn.net/2018060617045235?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)





以上图就是 首先找到的是 0节点 一开始权值是0，所以res +=0；然后暴力所有能够连接的点就是 1，和2， 然后更新 mincost[2]=min(INF,2)，结果等于2；同理 mincost[1]=10;

其它点的权值不变。 然后又开始找找到 2 顶点，然后暴力所有点，这次更新的就是3 4 5顶点。然后不断重复就行了

最后连接起来的树是这个样子的。

![](https://img-blog.csdn.net/20180606170506139?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)



下面是代码




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

using namespace std;
typedef long long ll;
typedef unsigned long long ull;

const long long mod=1e9+7;
const int maxv=1e3+25;          //最大顶点数
const int INF=0x7fffffff;
const int inf=0x3f3f3f3f;
const double eps=1e-8;


int cost[maxv][maxv];   // cost[u][v] 表示  u 到 v 的权值如果没有边权值为无穷大（INF）;
int mincost[maxv];     //每个点的最小权值，起点自己为0，其它为自己连接到集合最小权值；
bool used[maxv];       //表示顶点 是否已经连接上；
int V;                  //顶点数目；

int prim() {
    for(int i=0; i<V; i++) {
        mincost[i]=INF;   //开始的时候全部初始化为无穷大。
        used[i]=false;    //初始化 ，全部没有连接。
    }
    mincost[0]=0;   //以  0 节点为起点开始连接。
    int res = 0;        //权值和
    while(1) {
        int v=-1;         //选择的节点，开始为-1，然后开始找已经确定是最小的距离
        for(int u=0; u<V; u++) {
            if(!used[u]&& (v == -1 || mincost[u]<mincost[v]))v=u;    //如果 v==-1或者不是最小距离的时候更新v。
        }
        if(v==-1)break; //只有在所有的点都已经加入集合 v==-1。跳出循环。
        used[v]=true;    //把顶点加入集合。
        res+=mincost[v]; //把边的长度加到结果里
        for(int u=0; u<V; u++) {
            mincost[u]=min(mincost[u],cost[v][u]);    //从当前点到其它点的距离如果比其他路短就更新。
        }
    }
    return res;

}

int main() {
    int m;
    scanf("%d%d",&V,&m);   //输入顶点数和边数。 注意定点是从 0 到V-1。

    for(int i=0; i<m; i++) {
        int u,v,w;
        scanf("%d%d%d",&u,&v,&w);
        cost[u][v]=cost[v][u]=w;
    }
    printf("%d\n",prim());


//下面是POJ  1258 主程序代码。
    /*
        while(cin>>V) {
            for(int i=0; i<V; i++) {
                for(int j=0; j<V; j++) {
                    cin>>cost[i][j];
                }
            }
            printf("%d\n",prim());
        }
    */

    return 0;
}
```


接下来是Kruskal 代码



这个算法其实和Prim 算法差距不大，这个是直接把边排序





1. 找到最小的边

2. 判断边的两个节点是不是连接到同一颗树上，是跳过，不是连接两个顶点的根节点，结果加上边。

3. 重复上面两步，直到连接N-1条边，因为N个顶点要N-1条边就能连接起来。



![](https://img-blog.csdn.net/2018060617045235?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  


对于这个图，首先就找到（2，3 ）这条边连起来，然后就是（4 5），然后（0 2），这时候有2棵树

（0 2 3 ）和（4 5）然后接着连 （2 5）（5 6 ）（1 4）就全部连接上了



代码


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

using namespace std;
typedef long long ll;
typedef unsigned long long ull;

const long long mod=1e9+7;
const int maxv=1e3+25;          //最大顶点数
const int maxm=1e6+25;          //最大边数
const int INF=0x7fffffff;
const int inf=0x3f3f3f3f;
const double eps=1e-8;

int V,m;                  //顶点数目；

struct edge {
    int u,v,cost;   //节点用来 保存每个边的情况
};

bool cmp(const edge  & a,const edge & b) {
    return a.cost<b.cost;          //用于排序，相当于重载小于号。
}

edge es[maxm];

int par[maxv];   //  par[i]==j.    i 的 根节点为  j;

int find(int x) {       //寻找根节点
    if(x==par[x])return x;          //如果根节点就是自己直接返回自己
    else return par[x]=find(par[x]);    //如果根节点不是自己，继续寻找自己上一个节点的根节点。
}

void unit(int x,int y) {
    x=find(x);          //找到 x 的根节点
    y=find(y);
    par[x]=y;           //把 x 的根节点 连接上 y.
}

void init(int n) {
    for(int i=0; i<=n; i++)par[i]=i; //初始化的时候根节点都是自己；
}
int Kruskal() {
    sort(es,es+m,cmp);    //按  边的权值排序；
    init(V);
    int res=0,se=0;   //se 保存边数。
    for(int i= 0; i<m; i++) {
        edge e =es[i];
        if(find(e.u)!=find(e.v)) {      //如果根节点不相同就连接
            unit(e.u,e.v);
            res+=e.cost;
            if(++se==V-1)return res;   //如果连了V-1条边就跳出；
        }
    }
    return res;
}

int main() {
    scanf("%d%d",&V,&m);   //输入顶点数和边数。 注意定点是从 0 到V-1。

    for(int i=0; i<m; i++) {
        int u,v,w;
        scanf("%d%d%d",&u,&v,&w);
        es[i].u=u;
        es[i].v=v;
        es[i].cost=w;
    }
    printf("%d\n",Kruskal());
 

 // POJ 1285 AC 主程序代码
 /*
    while(cin>>V) {
        int k=0;
        for(int i=0; i<V; i++) {
            for(int j=0; j<V; j++) {
                cin>>es[k].cost;
                es[k].u=i;
                es[k].v=j;
                k++;
            }
        }
        m=k;
        printf("%d\n",Kruskal());
    }
*/
    return 0;
}

```
  
  
   
 