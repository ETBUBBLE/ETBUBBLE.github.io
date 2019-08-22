---
title: 2019 Multi-University Training Contest 10 1011 Make Rounddog Happy
date: 2019-08-22 15:37:48
tags:
    - 启发式分治
categories:
    - ACM
    - 分治
mathjax: true
---
[HDU 6701 Make Rounddog Happy](http://acm.hdu.edu.cn/showproblem.php?pid=6701)
**题意 ：** 给你$n$个数，和$k$,找到区间`[l,r]` $max(a_l,\dots,a_r)-(r-l+1)<=k$ 的数量（区间内不能出现有相同数字）。
**题解：** 相当于找`区间长度`大于`区间最大值-k` 的区间数量，第一个想到入手的肯定就是区间最大值，然后枚举左边或右边的边界，然后找另外一边的的上界。
假设寻找区间最大值，st表能够实现$O(1)$的查找，假设我们找到`[l,r]`区间的最大值下标为`MID`，这个`MID`很显然不会恰好在正中间，那么我们肯定是向里边界近的方向枚举，然后查询另一端的情况。**(这个叫启发式分治 队友告诉我是$O(nlog(n))$)** 。假设枚举右端下标为当前下标`i`,另一端的情况怎么获取呢，$O(n)$找肯定不行，另一端上界`up`肯定是$i-a[MID]-k$,下届呢？从`i`开始第一个出现相同数字的位置。这个可以$O(n)$ 预处理出来，每个位置上的数上一次出现的位置可以直接求出来，然后从某一个位置到第一个出现相同值的下标肯定是单调的，所以能$O(n)$预处理出第一个从`i`位置往前最长不出现相同值的位置，和向后最长不出现相同值的位置 **这个地方卡了点常，我用2个ST表卡极限时间过了，实际上两个数组就能解决，找区间最大值实际上也能用单调栈解决可以不用ST表**。画个图理解一下
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190822153349406.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)
如果是，枚举左边一样的处理。
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
const int maxn = (int) 3e5 + 5;
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

void read(int &w) {//读入
    char c;
    while (!isdigit(c = getchar()));
    w = c & 15;
    while (isdigit(c = getchar()))
        w = w * 10 + (c & 15);
}

void output(LL x) {
    if (x < 0)
        putchar('-'), x = -x;
    int ss[55], sp = 0;
    do
        ss[++sp] = x % 10;
    while (x /= 10);
    while (sp)
        putchar(48 + ss[sp--]);
}

const int MXN = 3e5 + 10;
int a[maxn];
int pre[maxn], nxt[maxn];
int dp[MXN][20], pos[MXN][20];

int lg[maxn];

void init(int n) {
    int LOG = lg[n] + 1;
    for (int j = 1; j < LOG; ++j) {
        if ((1 << (j - 1)) > n) break;
        for (int i = 1; i + (1 << j) - 1 <= n; ++i) {
            if (dp[i][j - 1] >= dp[i + (1 << (j - 1))][j - 1]) {
                dp[i][j] = dp[i][j - 1];
                pos[i][j] = pos[i][j - 1];
            } else {
                dp[i][j] = dp[i + (1 << (j - 1))][j - 1];
                pos[i][j] = pos[i + (1 << (j - 1))][j - 1];
            }
        }
    }
}

inline int query(int l, int r) {
    int k = lg[r - l + 1];
    if (dp[l][k] >= dp[r - (1 << k) + 1][k]) return pos[l][k];
    return pos[r - (1 << k) + 1][k];
}

inline int query1(int l, int r) {
    return pre[r];
}

inline int query2(int l, int r) {
    return nxt[l];
}

int n, k;
LL ans = 0;

void solve(int l, int r) {
    if (l > r) return;
    if (l == r) {
        if (a[l] - 1 <= k) ++ans;
        return;
    }
    int MID = query(l, r);//最大值的位置
    if (r - MID > MID - l) {
        int up = min(query2(MID, r) - 1, r), low;
        for (int i = MID; i >= l; --i) {//枚举左端点
            up = min(nxt[i] - 1, up);
            low = i + (a[MID] - k) - 1;
            low = max(low, MID);
            if (up < MID)break;
            if (low > up)continue;
            else {
                ans += up - low + 1;
            }
        }
    } else {
        int up, low = max(query1(l, MID) + 1, l);
        for (int i = MID; i <= r; ++i) {//枚举右端点
            low = max(pre[i] + 1, low);
            up = i - (a[MID] - k) + 1;
            up = min(up, MID);
            if (low > MID)break;
            if (low > up)continue;
            else {
                ans += up - low + 1;
            }
        }
    }
    solve(l, MID - 1);
    solve(MID + 1, r);
}

int p[maxn];

int main() {
    f();
    int T;
    read(T);
    for (int i = 1; i <= 3e5; i++) {
        lg[i] = log2(i);
    }
    while (T--) {
        read(n);
        read(k);
        for (int i = 1; i <= n; i++) {
            read(a[i]);
            dp[i][0] = a[i];
            pos[i][0] = i;
        }
        for (int i = 1; i <= n; i++) {
            p[i] = 0;
        }
        for (int i = 1; i <= n; i++) {
            pre[i] = p[a[i]];
            if (i != 1)pre[i] = max(pre[i], pre[i - 1]);
            p[a[i]] = i;
        }
        for (int i = 1; i <= n; i++) {
            p[i] = n + 1;
        }
        for (int i = n; i >= 1; i--) {
            nxt[i] = p[a[i]];
            if (i != n)nxt[i] = min(nxt[i], nxt[i + 1]);
            p[a[i]] = i;
        }
        for (int i = 1; i <= n; ++i) {
//            dp1[i][0] = pre[i];
//            dp2[i][0] = nxt[i];
        }
        init(n);
        ans = 0;
        solve(1, n);
        output(ans);
        puts("");
    }
    return 0;
}
```

---