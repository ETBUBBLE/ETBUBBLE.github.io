---
title: 2019牛客暑期多校训练营(第八场)Flower Dance(有坑)
date: 2019-08-13 21:31:28
tags:
    - 线段树
    - 离散化
    - 并查集
    - 牛客暑期多校训练营
categories:
    - ACM
    - 数据结构
mathjax: true
---
[2019牛客暑期多校训练营(第八场)Flower Dance](https://ac.nowcoder.com/acm/contest/888/F)
** 题意: ** 给$n$个点 $m$条边，每条边有一个权值区间，表示能通过这个区间的 值的范围，问从$1$到$n$可以通过的权值有多少个。
** 题解: ** 
## 1.DFS线段树+离散化+并查集
这个线段树，其实也不能算是个正常的线段树，他`build` 的之后就没啥用了，没有更新和查询.。。直接在线段树上`dfs`,首先把权值离散化，然后存入线段树中，线段树每个节点表示，区间`[l,r]`，中有哪些边，这样每次深搜下去，经过的边用并查集维护起来，表示哪些点是联通的，然后如果，`1`和`n`联通的话就更新权值。这题有回溯，就有拆边，所以并查集要保存路径，而且要加`按秩合并`的优化,不然会超时。
** 写离散化线段树尽量保存 `(l,r]` 左开右闭的区间值，保存`[l,r]` 的区间会出问题**

```c
#include "bits/stdc++.h"

using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
typedef pair<int, int> P;
#define VNAME(value) (#value)
#define bug printf("*********\n");
#define debug(x) cout<<"["<<VNAME(x)<<" = "<<x<<"]"<<endl;
#define mid ((l + r) >> 1)
#define chl 2 * k + 1
#define chr 2 * k + 2
#define lson l, mid, chl
#define rson mid, r, chr
#define eb(x) emplace_back(x)
#define pb(x) emplace_back(x)
#define mem(a, b) memset(a, b, sizeof(a));

const LL mod = (LL) 1e9 + 7;
const int maxn = (int) 1e6 + 5;
const LL INF = 0x7fffffff;
const LL inf = 0x3f3f3f3f;
const double eps = 1e-8;

#ifndef ONLINE_JUDGE
clock_t prostart = clock();
#endif

void f() {
#ifndef ONLINE_JUDGE
    freopen("../data.in", "r", stdin);
#endif
}

//typedef __int128 LLL;

template<typename T>
void read(T &w) {//读入
    char c;
    while (!isdigit(c = getchar()));
    w = c & 15;
    while (isdigit(c = getchar()))
        w = w * 10 + (c & 15);
}

template<typename T>
void output(T x) {
    if (x < 0)
        putchar('-'), x = -x;
    int ss[55], sp = 0;
    do
        ss[++sp] = x % 10;
    while (x /= 10);
    while (sp)
        putchar(48 + ss[sp--]);
}

struct edge {
    int u, v, l, r;
} eg[maxn];


vector<int> v;

int get(int x) {
    return lower_bound(v.begin(), v.end(), x) - v.begin() + 1;
}

int n, m;
vector<int> dat[maxn];

void build(int l, int r, int k, int a, int b, int x) {
    if (l == a && b == r) {
        dat[k].emplace_back(x);
        return;
    }
    if (b <= mid) {
        build(lson, a, b, x);
    } else if (a >= mid) {
        build(rson, a, b, x);
    } else {
        build(lson, a, mid, x);
        build(rson, mid, b, x);
    }
}

vector<int> d[maxn];
int ans = 0;
int par[maxn];

int find(int x) {
    return (par[x] == x) ? x : find(par[x]);
}

int rk[maxn];

void unit(int x, int y, int dep) {
    x = find(x);
    y = find(y);
    if (x != y) {
        if (rk[x] < rk[y])swap(x, y);
        par[y] = x;
        d[dep].emplace_back(y);
        if (rk[x] == rk[y])rk[x]++;
    }
}

void dfs(int l, int r, int k, int dep) {
    d[dep].clear();
    for (auto au:dat[k]) {
        unit(eg[au].u, eg[au].v, dep);
    }
    if (find(1) == find(n)) {
        ans += v[r - 1] - v[l - 1];
    } else if (r != l + 1) {
        dfs(lson, dep + 1);
        dfs(rson, dep + 1);
    }
    for (auto au:d[dep]) {
        par[au] = au;
    }
}

int main() {
    f();
    read(n);
    read(m);
    for (int i = 0; i <= n; i++)par[i] = i;
    for (int i = 0; i < m; i++) {
        read(eg[i].u);
        read(eg[i].v);
        read(eg[i].l);
        read(eg[i].r);
        v.emplace_back(eg[i].l);
        v.emplace_back(eg[i].r + 1);
    }
    sort(v.begin(), v.end());
    v.erase(unique(v.begin(), v.end()), v.end());
    int mx = v.size();
    for (int i = 0; i < m; i++) {
        build(1, mx, 1, get(eg[i].l), get(eg[i].r + 1), i);
    }
    dfs(1, mx, 1, 0);
    output(ans);
    puts("");

#ifndef ONLINE_JUDGE
    cout << "运行时间:" << 1.0 * (clock() - prostart) / CLOCKS_PER_SEC << endl;
#endif
    return 0;
}

```
## LCT+最小生成树 挖个坑，懒得学splay