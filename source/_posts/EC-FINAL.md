---
title: EC-FINAL I Misunderstood Missing
date: 2019-04-25 20:40:39
tags:
    - dp
categories:
    - dp
---
## 写此篇记录EC-FINAL 铁牌。 ##
### I [Misunderstood … Missing](https://ac.nowcoder.com/acm/contest/366/I)  ###
此题，是EC打铁的主要原因。在牛客的那个网站上此题出的比F题还要多，然而我在比赛中却没有写出来，如果这题写出来了，可能还有救。对于这题DP没写出来，感觉是真的傻逼。
以后打比赛切记不可以慌张，慌张并不能解决问题，这题如果静下心来思考应该不难想出来。
题意：t 组数据 n 个回合
初始战力 A 为 0 后面有n 个回合，每个回合可以进行一个操作
1可以造成A+a点伤害
2每个回合战力加b
3直接提升 c 点战斗力
问最多能造成多少点伤害
题解：DP求解 dp[i][j]为j 个回合进行了第一种操作，操作的下标和为j。
如果把1 2 3 三种操作的伤害区分开来，1 操作就是直接造成 a 点伤害。 2 操作会对后面的A造成影响，仔细思考一下加入 2 操作是 第 i 个 后面进行了 j  次攻击 ,能够发现  2  操作 最终造成的伤害和是 `（下标和 - i * j ） b` 点伤害。举个例子：如果 你在 2 3 进行了攻击,你在第1个回合进行了2操作 那么贡献就 是 `(2+3 - 1 * 2)*b`。理解了这个 3 操作就更简单了，直接就是`j*c`的伤害。
看到这里也差不多明白，这个DP是从后往前DP的。后面的状态永远都不会影响到前面。
```
#include "bits/stdc++.h"

using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
typedef pair<int, int> P;

#define bug printf("*********\n");
#define debug(x) cout << "[" << x << "]" << endl;
#define mid (l + r) / 2
#define chl 2 * k + 1
#define chr 2 * k + 2
#define lson l, mid, chl
#define rson mid + 1, r, chr
#define pb push_back
#define mem(a, b) memset(a, b, sizeof(a));

const long long mod = 1e9 + 7;
const int maxn = 6e3 + 5;
const int INF = 0x7fffffff;
const LL inf = 0x3f3f3f3f;
const double eps = 1e-8;

void f() {
#ifndef ONLINE_JUDGE
    freopen("../data.in", "r", stdin);
#endif
    return;
}

LL dp[maxn][105];
LL a[105], b[105], c[105];

int main() {
    f();
    int t, n;
    scanf("%d", &t);
    while (t--) {
        scanf("%d", &n);
        for (int i = 1; i <= n; i++) {
            scanf("%lld%lld%lld", &a[i], &b[i], &c[i]);
        }
        memset(dp, -1, sizeof(dp));
        dp[n][1]=a[n];
        for (int i = n - 1; i >= 1; i--) {
            for (int j = maxn-1; j>=0; j--) {
                for (int l = 0; l <= n; l++) {
                    if(dp[j][l]==-1)continue;
                    if(j+i<maxn)dp[j+i][l+1]=max(dp[j+i][l+1],dp[j][l]+a[i]);
                    dp[j][l]=max(dp[j][l]+c[i]*l,max(dp[j][l],dp[j][l]+b[i]*(j-l*i)));
                }
            }
        }
        LL ans=0;
        for(int i=0; i<=n; i++) {
            for(int j=0; j<maxn; j++) {
                ans=max(dp[j][i],ans);
            }
        }
        printf("%lld\n",ans);
    }
    return 0;
}

```
