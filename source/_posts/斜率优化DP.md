---
title: 斜率优化DP
date: 2019-08-23 19:21:19
tags:
    - 斜率优化DP
categories:
    - ACM
    - DP
mathjax: true
---
什么是斜率优化DP？顾名思义，用斜率优化的DP。
推荐一波[博客](https://www.cnblogs.com/orzzz/p/7885971.html)这个大哥将的不错。
斜率优化DP，一开始会化成一个式子，像
$$
   \frac{f(j)-f(k)}{g(j)-g(k)}<s(i)
$$
满足这个式子可以得到$j$转移比$x$转移要优，其中不出意外，$s(i)$是递增的。把这个式子画在图上就是
![](https://i.loli.net/2019/08/23/jhD1QNaRi9dYGop.png)
$j,k,l$向$i$转移$j$优于$l$,$k$优于$j$。
利用这个性质我们，保存一个斜率递增的优先队列。，然后队首第一个节点就是转移的最优解。
![](https://i.loli.net/2019/08/23/oJYxLQyzqAhpNf7.png)
不难看出上图，$k$点最优。
学习例题: [HDU 3507](http://acm.hdu.edu.cn/showproblem.php?pid=3507)
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

LL a[maxn];
LL s[maxn];
LL dp[maxn];

LL getup(int i, int j) {
    return dp[i] + s[i] * s[i] - dp[j] - s[j] * s[j];
}

LL getdown(int i, int j) {
    return s[i] - s[j];
}

int n, m;

LL getdp(int i, int j) {
    return dp[j] + (s[i] - s[j]) * (s[i] - s[j]) + m;
}

int p[maxn];

int main() {
    f();
    while (scanf("%d%d", &n, &m) != EOF) {
        mem(dp, 0);
        for (int i = 1; i <= n; i++) {
            read(a[i]);
            s[i] = a[i] + s[i - 1];
        }
        int head = 0, tail = 0;
        tail++;
        dp[0] = 0;
        for (int i = 1; i <= n; i++) {
            while (head + 1 < tail && getup(p[head + 1], p[head]) < getdown(p[head + 1], p[head]) * 2 * s[i])head++;
            dp[i] = getdp(i, p[head]);
            while (head + 1 < tail && getup(p[tail - 1], p[tail - 2]) * getdown(i, p[tail - 1]) >=
                                      getup(i, p[tail - 1]) * getdown(p[tail - 1], p[tail - 2]))
                tail--;
            p[tail++] = i;
        }
        printf("%lld\n", dp[n]);
    }
#ifndef ONLINE_JUDGE
    cout << "运行时间:" << 1.0 * (clock() - prostart) / CLOCKS_PER_SEC << endl;
#endif
    return 0;
}

```

---

进阶题: [J	Wood Processing](https://ac.nowcoder.com/acm/contest/890/J) [题解](https://www.etbubble.xyz/2019/08/23/2019%E7%89%9B%E5%AE%A2%E6%9A%91%E6%9C%9F%E5%A4%9A%E6%A0%A1%E8%AE%AD%E7%BB%83%E8%90%A5%EF%BC%88%E7%AC%AC%E5%8D%81%E5%9C%BA%EF%BC%89J%20Wood%20Processing/#more)