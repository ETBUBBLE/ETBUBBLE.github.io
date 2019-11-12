---
title: 2019牛客暑期多校训练营(第十场) J	Wood Processing
date: 2019-08-23 19:43:04
tags:
    - 斜率优化DP
categories:
    - ACM
    - DP
mathjax: true
---
**题意:** $n$快木板，合成$k$块木板。相同高度可以合并，不同高度，需要把高的木板砍成和低的木板一样高。问合成$k$块最少浪费多大木板面积。
**题解:** `dp[i][j]` 表示前`j`个合成`i`个木板最小花费面积。转移方程为
$$
dp[i][j]=min(dp[i-1][k]+sum[j]-sum[k]+h[k+1] * (w[j]-w[k]),dp[i][j])
$$
其中$w$表示表示宽度前缀和，$sum$表示面积前缀和，转移的贡献就是，

假设从$k$转移优于$l$，有如下:
$$
dp[i-1][k]+sum[j]-sum[k]-h[k+1] * (w[j]-[k])=dp[i-1][l]+sum[j]-sum[l]+h[l+1] * (w[j]-w[l])
$$
合并同类项化简，
$$
\frac{(dp[i-1][k]-sum[k]+h[k+1] * w[k])-(dp[i-1][l]-sum[l]+h[l+1] * w[l])}{w[k]-w[l]}<w[j]
$$
典型的斜率优化DP

```c
#include "bits/stdc++.h"

using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
typedef __int128 LLL;
typedef pair<LLL, LLL> P;
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
const int maxn = (int) 5e3 + 5;
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


LLL dp[maxn][maxn];

P dat[maxn];

LLL sum[maxn];
LLL w[maxn];

LLL getup(int i, int j, int k) {
    return (dp[i - 1][j] - sum[j] + w[j] * dat[j + 1].first) -
           (dp[i - 1][k] - sum[k] + w[k] * dat[k + 1].first);
}

LLL getdown(int j, int k) {
    return dat[j + 1].first - dat[k + 1].first;
}

LLL getdp(int i, int j, int k) {
    return dp[i - 1][k] + sum[j] - sum[k] - dat[k + 1].first * (w[j] - w[k]);
}

int p[maxn];

int main() {
    f();
    int n, k;
    read(n);
    read(k);
    for (int i = 1; i <= n; i++) {
        read(dat[i].second);
        read(dat[i].first);
    }
    sort(dat + 1, dat + n + 1);
    for (int i = 1; i <= n; i++) {
        sum[i] = sum[i - 1] + dat[i].second * dat[i].first;
        w[i] = w[i - 1] + dat[i].second;
    }
    mem(dp, inf);
    dp[0][0] = 0;
    for (int i = 1; i <= k; i++) {
        int head = 0, tail = 1;
        for (int j = 1; j <= n; j++) {
            while (head + 1 < tail && getup(i, p[head + 1], p[head]) <= getdown(p[head + 1], p[head]) * w[j])head++;
            dp[i][j] = getdp(i, j, p[head]);
            while (head + 1 < tail && getup(i, p[tail - 1], p[tail - 2]) * getdown(j, p[tail - 1]) >
                                      getup(i, j, p[tail - 1]) * getdown(p[tail - 1], p[tail - 2]))
                tail--;
            p[tail++] = j;
        }
    }
    output(dp[k][n]);
    puts("");
#ifndef ONLINE_JUDGE
    cout << "运行时间:" << 1.0 * (clock() - prostart) / CLOCKS_PER_SEC << endl;
#endif
    return 0;
}
```

---
