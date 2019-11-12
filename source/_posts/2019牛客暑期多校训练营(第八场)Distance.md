---
title: 2019牛客暑期多校训练营(第八场) Distance
date: 2019-08-13 21:09:16
tags:
    - 树状数组
    - 分块
    - 牛客暑期多校训练营
categories:
    - ACM
    - 数据结构
mathjax: true
---
[2019牛客暑期多校训练营(第八场) Distance](https://ac.nowcoder.com/acm/contest/888/D)
**题意:** 给你一个 $n\* m\*  h$ 的空间，每次插入一个点，或者询问空间中点到这一点的最小曼哈顿距离。
**题解:**
## **1.HASH+三维BIT**
三维BIT，对于这种写法，太巨了，$n \* m \* h < 1e5$ 特么直接用三维BIT 存一下就可以了，枚举八个方向，把绝对值去掉，然后最牛的还是`hash`处理，把三维压缩成一维，削常数，我特么卡常卡成这样也是少见。
**这种`hash`将维度，枚举八个方向去绝对值，数组数组模拟空间，还是挺6的**
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
#define rson mid + 1, r, chr
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

typedef vector<int> VI;
typedef vector<VI> VVI;
typedef vector<VVI> VVVI;

struct BIT {
    int n, m, h;
    int a[maxn];

    void init(int n, int m, int h) {
        this->n = n;
        this->m = m;
        this->h = h;
        mem(a, -inf);
    }

    int get(int x, int y, int z) {
        return x * h * m + y * h + z;
    }

    int lowbit(int &x) {
        return x & (-x);
    }

    void add(int x, int y, int z, int t) {
        for (int i = x; i <= n; i += lowbit(i))
            for (int j = y; j <= m; j += lowbit(j))
                for (int k = z; k <= h; k += lowbit(k)) a[get(i, j, k)] = max(a[get(i, j, k)], t);
    }

    int sum(int x, int y, int z) {
        int res = -inf;
        for (int i = x; i; i -= lowbit(i))
            for (int j = y; j; j -= lowbit(j))
                for (int k = z; k; k -= lowbit(k))
                    res = max(a[get(i, j, k)], res);
        return res;
    }
} b[8];

int get_pos(int x, int pos) {
    return (x >> pos) & 1;
}

int main() {
    f();
    int n, m, h, q;
    int op;
    read(n);
    read(m);
    read(h);
    read(q);
    for (int i = 0; i < 8; i++) {
        b[i].init(n, m, h);
    }
    int d[] = {n + 1, m + 1, h + 1};
    int a[3], c[3];
    while (q--) {
        read(op);
        read(a[0]);
        read(a[1]);
        read(a[2]);
        if (op == 1) {
            for (int i = 0; i < 8; i++) {
                c[0] = get_pos(i, 0) ? d[0] - a[0] : a[0];
                c[1] = get_pos(i, 1) ? d[1] - a[1] : a[1];
                c[2] = get_pos(i, 2) ? d[2] - a[2] : a[2];
                b[i].add(c[0], c[1], c[2], c[1] + c[2] + c[0]);
            }
        } else {
            int ans = inf;
            for (int i = 0; i < 8; i++) {
                c[0] = get_pos(i, 0) ? d[0] - a[0] : a[0];
                c[1] = get_pos(i, 1) ? d[1] - a[1] : a[1];
                c[2] = get_pos(i, 2) ? d[2] - a[2] : a[2];
                ans = min(ans, c[0] + c[1] + c[2] - b[i].sum(c[0], c[1], c[2]));
            }
            output(ans);
            puts("");
        }
    }
#ifndef ONLINE_JUDGE
    cout << "运行时间:" << 1.0 * (clock() - prostart) / CLOCKS_PER_SEC << " s" << endl;
#endif
    return 0;
}

```

---

## 2.分块+bfs
第二种写法就更骚了Orz,对于 $1e5$ 组查询，分成$\sqrt{(1e5)}$ 块，每次插入一个点，先判断有没有$\sqrt{(1e5)}$个，少于这个数量，直接暴力找，假设就算每次都是满的都是 $\sqrt{(1e5)} \* 1e5$ 的复杂度，然后如果满了，直接空间中`bfs`,比如说你插入了两个点,`0,0,1` `0,0,2`,直接暴力`bfs`,枚举`6`个方向,找离这个点最近的距离是多少。暴力枚举空间复杂度是$O(n \* m \* h)$.总复杂度就是 $q\*\sqrt{q}+\sqrt{n \* m \* h} \* q$.
** 这种分块更新的操作，学不来，学不来，根本学不来。 **

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
#define rson mid + 1, r, chr
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
 
int a[maxn];
 
int dir[6][3] = {{1,  0,  0},
                 {-1, 0,  0},
                 {0,  1,  0},
                 {0,  -1, 0},
                 {0,  0,  1},
                 {0,  0,  -1}};
int X[maxn], Y[maxn], Z[maxn];
int lx, ly, lz;
int n, m, h, q;
 
int get(int x, int y, int z) {
    return x * h * m + y * h + z;
}
 
 
struct node {
    int k, x, y, z;
 
    node(int x, int y, int z) {
        this->x = x;
        this->y = y;
        this->z = z;
        this->k = get(x, y, z);
    }
};
 
void rebuild() {
    queue<node> que;
    for (int i = 0; i < lx; i++) {
        que.push(node(X[i], Y[i], Z[i]));
        a[get(X[i], Y[i], Z[i])] = 0;
    }
    while (que.size()) {
        node t = que.front();
        que.pop();
        for (int i = 0; i < 6; i++) {
            int tox = t.x + dir[i][0], toy = t.y + dir[i][1],
                    toz = t.z + dir[i][2];
            if (tox < 0 || tox >= n || toy < 0 || toy >= m || toz < 0 || toz >= h) continue;
            int k = get(tox, toy, toz);
            if (a[k] > a[t.k] + 1) {
                a[k] = a[t.k] + 1;
                que.push(node(tox, toy, toz));
            }
 
        }
    }
    lx = 0;
    ly = 0;
    lz = 0;
}
 
int op, x, y, z;
 
int main() {
    f();
    mem(a, inf);
    read(n);
    read(m);
    read(h);
    read(q);
    while (q--) {
        read(op);
        read(x);
        read(y);
        read(z);
        --x, --y, --z;
        if (op == 1) {
            X[lx++] = x;
            Y[ly++] = y;
            Z[lz++] = z;
        } else {
            int k = get(x, y, z);
            int ans = a[k];
            for (int i = 0; i < lx; i++) {
                ans = min(ans, abs(x - X[i]) + abs(z - Z[i]) + abs(y - Y[i]));
            }
            output(ans);
            puts("");
        }
        if (lx == 300) {
            rebuild();
        }
    }
#ifndef ONLINE_JUDGE
    cout << "运行时间:" << 1.0 * (clock() - prostart) / CLOCKS_PER_SEC << " s" << endl;
#endif
    return 0;
}
```


