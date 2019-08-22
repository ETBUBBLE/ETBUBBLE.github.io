---
title: 2019 Multi-University Training Contest 1 1001 Blank
date: 2019-07-27 20:04:06
tags:
    - DP
    - 杭电多校
categories:
    - ACM
    - 动态规划
mathjax: true
---
[HDU 6578 Blank](http://acm.hdu.edu.cn/showproblem.php?pid=6578)
**题意：** 给定`1,N` 的位置，每个位置可以填`1,2,3,4`其中一个，给`m`个区间`[l,r] x` ，限制`[l,r]`区间内只有`x`种不同的数。
**题解：** `n`非常小，只有100，可以直接用数组枚举上一个数出现的位置，每个位置暴力填就行了。直接$O(n^4)$会T，。，必须要削常数。可以发现出现是什么数本身不重要，只和位置有关。然后最大的那个位置，一定是你要填的这个`pos-1`,所以`dp`空间可以优化一维，`dp[i][j][k]`,代表排序后位置分别是`i<j<k<pos-1`，然后枚举的状态也是`i<j<k<pos-1` ，对于限制条件，判断一下状态合不合法就行了。对于某个`pos`到了`[l,r] x`,`r`的位置,判断一下就行了，这个不影响复杂度。
```c
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
#include<bitset>
#include<unordered_map>
#include<unordered_set>

using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
typedef pair<int, int> P;

#define VNAME(value) (#value)
#define bug printf("*********\n");
#define debug(x) cout<<"["<<VNAME(x)<<" = "<<(x)<<"]"<<endl;
#define mid (l+r)/2
#define chl 2*k+1
#define chr 2*k+2
#define lson l,mid,chl
#define rson mid+1,r,chr
#define pb push_back
#define mem(a, b) memset(a,b,sizeof(a));

const long long mod = 998244353;
const int maxn = 1e6 + 5;
const int INF = 0x7fffffff;
const LL inf = 0x3f3f3f3f;
const double eps = 1e-8;

void f() {
#ifndef ONLINE_JUDGE
    freopen("../data.in", "r", stdin);
#endif // ONLIN_JUDGE
}

LL dp[105][105][105][2];

struct node {
    int l, r, x;

    bool operator<(node &t) const {
        if (r == t.r)return l < t.l;
        return r < t.r;
    }
} dat[maxn];

int n, m;

void check(LL &x) {
    x %= mod;
}

int main() {
    f();
    int T;
    scanf("%d", &T);
    while (T--) {
        scanf("%d%d", &n, &m);
        for (int i = 1; i <= m; i++) {
            scanf("%d%d%d", &dat[i].l, &dat[i].r, &dat[i].x);
            dat[i].l += 3;
            dat[i].r += 3;
        }
        dp[0][1][2][1] = 1;
        sort(dat + 1, dat + m + 1);
        int l = 1, p = 0;
        for (int pos = 4; pos <= n + 3; pos++, p = !p) {
            for (int i = 0; i <= pos - 4; i++) {
                for (int j = i + 1; j <= pos - 3; j++) {
                    for (int k = j + 1; k <= pos - 2; ++k) {
                        int flag = 0, temp = l;
                        while (temp <= m && pos - 1 == dat[temp].r) {
                            if (dat[temp].x == 1 && dat[temp].l <= k) {
                                flag = 1;
                            }
                            if (dat[temp].x == 2 && (dat[temp].l <= j || dat[temp].l > k)) {
                                flag = 1;
                            }
                            if (dat[temp].x == 3 && (dat[temp].l <= i || dat[temp].l > j))flag = 1;
                            if (dat[temp].x == 4 && dat[temp].l > i)flag = 1;
                            temp++;
                        }
                        if (flag) {
                            dp[i][j][k][!p] = 0;
                            continue;
                        }
                        dp[j][k][pos - 1][p] += dp[i][j][k][!p];
                        check(dp[j][k][pos - 1][p]);
                        dp[i][k][pos - 1][p] += dp[i][j][k][!p];
                        check(dp[i][j][pos - 1][p]);
                        dp[i][j][pos - 1][p] += dp[i][j][k][!p];
                        check(dp[i][j][pos - 1][p]);
                        dp[i][j][k][p] += dp[i][j][k][!p];
                        check(dp[i][j][k][p]);
                        dp[i][j][k][!p] = 0;
                    }
                }
            }
            while (l <= m && dat[l].r == pos - 1)l++;
        }
        LL ans = 0;
        for (int i = 0; i <= n + 3; i++) {
            for (int j = i + 1; j <= n + 3; j++) {
                for (int k = j + 1; k <= n + 3; ++k) {
                    int flag = 0, temp = l;
                    while (temp <= m) {
                        if (dat[temp].x == 1 && dat[temp].l <= k) {
                            flag = 1;
                        }
                        if (dat[temp].x == 2 && (dat[temp].l <= j || dat[temp].l > k)) {
                            flag = 1;
                        }
                        if (dat[temp].x == 3 && (dat[temp].l <= i || dat[temp].l > j))flag = 1;
                        if (dat[temp].x == 4 && dat[temp].l > i)flag = 1;
                        temp++;
                    }
                    if (flag) {
                        dp[i][j][k][!p] = 0;
                        continue;
                    }
                    ans = (ans + dp[i][j][k][!p] % mod) % mod;
                    dp[i][j][k][!p] = 0;
                }
            }
        }
        printf("%lld\n", ans);
    }
    return 0;
}
/*
6
1 0
4
10 0
1048576
11 0
4194304
20 0
444595123
30 0
682155965
40 0
382013690
 */
```

---