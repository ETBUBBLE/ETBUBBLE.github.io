---
title: 2019 Multi-University Training Contest 3 1011 Squrirrel
date: 2019-07-30 10:20:55
tags:
    - 树形DP
    - 杭电多校
categories:
    - ACM
    - 动态规划
---
[HDU 6613 Squrirrel](http://acm.hdu.edu.cn/showproblem.php?pid=6613)
**题意：** 可以合并树上两个点，合并两个点让某一个点到离他最远的距离最小，如果有多个答案输出字典序最小的。
**题解：** 首先从叶子节点往根节点跑，保存每个到这个点的最大距离，和儿子节点删掉一条边之后最大距离的最小值。（肯定是从最大路径上删一条边）~~我为了保险全判断了~~ 。然后再从根节点往儿子节点跑，每次保存一个，最大，次大，第三大，具体为什么看代码注释。
```c
#include "bits/stdc++.h"

using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
typedef pair<int, int> P;
#define VNAME(value) (#value)
#define bug printf("*********\n");
#define debug(x) cout<<"["<<VNAME(x)<<" = "<<x<<"]"<<endl;
#define mid (l + r) / 2
#define chl 2 * k + 1
#define chr 2 * k + 2
#define lson l, mid, chl
#define rson mid + 1, r, chr
#define pb push_back
#define mem(a, b) memset(a, b, sizeof(a));


const LL mod = (LL) 1e9 + 7;
const int maxn = (int) 2e5 + 5;
const int INF = 0x7fffffff;
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

int ans1;

vector<int> G[maxn];
vector<int> cost[maxn];
int dp[maxn];
int mi[maxn];
int in[maxn];
int used[maxn];
int ans2 = inf;


void dfs(int r, int p, int mx, int cmi, int c) {
    cmi = min(cmi + c, mx);
    mx = mx + c;
    int temp = min(max(mx, mi[r]), max(cmi, dp[r]));

    if (ans2 > temp) {
        ans2 = temp;
        ans1 = r;
    }
    if (ans2 == temp) {
        ans2 = temp;
        ans1 = min(ans1, r);
    }

//    cout << "r=" << r << "mx=" << mx << "mi=" << cmi << endl;
//    int temp = min(max(dp[r], cmi), max(mi[r], mx));
//    if (ans2 > temp) {
//        ans2 = min(max(dp[r], cmi), max(mi[r], mx));
//        ans1 = r;
//    } else if (ans2 == temp)ans1 = min(r, ans1);
    int mx1 = mx, mx2 = 0, mx3 = 0, mi2 = -1, mi1 = p, mi3 = -1, vmi1 = cmi, vmi2 = 0, vmi3 = 0; 
    //记录最大，次大，第三大
    for (int i = 0; i < G[r].size(); i++) {
        int au = G[r][i];
        int c = dp[au] + cost[r][i];
        if (au == p) continue;
        temp = min(dp[au], mi[au] + cost[r][i]);
        if (mx1 <= c) {
            swap(mx1, c);
            swap(mi1, au);
            swap(vmi1, temp);
        }
        if (mx2 <= c) {
            swap(mx2, c);
            swap(mi2, au);
            swap(vmi2, temp);
        }
        if (mx3 <= c) {
            swap(mx3, c);
            swap(mi3, au);
            swap(vmi3, temp);
        }
    }
    for (int i = 0; i < G[r].size(); i++) {
        int au = G[r][i];
        int c = cost[r][i];
        if (au == p)continue;
        if (au == mi1) {
            if (mi3 == -1)dfs(au, r, mx2, vmi2, c);
            else dfs(au, r, mx2, max(vmi2, mx3), c);  //如果是往最大的路径走，就是在次大的路上删一条边再和第三大比较
        } else if (au == mi2) {
            if (mi3 == -1)dfs(au, r, mx1, vmi1, c);
            else dfs(au, r, mx1, max(vmi1, mx3), c); //如果是往第二大的，就是在最大路上删一条边再和第三大比较
        } else dfs(au, r, mx1, max(vmi1, mx2), c);  // 其他的肯定都是删最大的路径一条边 再和第二大比较
    }
}

int main() {
    f();
    int n;
    int t;
    scanf("%d", &t);
    while (t--) {
        scanf("%d", &n);
        for (int i = 1; i < n; i++) {
            int u, v, c;
            scanf("%d%d%d", &u, &v, &c);
            G[u].emplace_back(v);
            cost[u].emplace_back(c);
            G[v].emplace_back(u);
            cost[v].emplace_back(c);
            in[u]++;
            in[v]++;
        }
        queue<int> q;
        for (int i = 1; i <= n; i++) {
            dp[i] = 0;
            if (in[i] == 1) {
                q.push(i);
                mi[i] = 0;
            } else mi[i] = inf;
        }
        int r = -1;
        ans1 = 0;
        ans2 = inf;
        while (q.size()) {
            int v = q.front();
            q.pop();
            r = v;
            if (used[v] == 0)used[v] = 1;
            else continue;
            int mx1 = 0, mx2 = 0;

            for (int i = 0; i < G[r].size(); i++) {
                int au = G[r][i];
                int c = dp[au] + cost[r][i];
                if (!used[au])continue;
                if (mx1 < c)swap(mx1, c);
                if (mx2 < c)swap(mx2, c);
            }
            for (int i = 0; i < G[r].size(); i++) {
                int au = G[r][i];
                int c = cost[r][i];
                if (!used[au])continue;
                if (dp[au] + c == mx1) {
                    mi[r] = min(mi[r], max(min(dp[au], mi[au] + c), mx2)); //这个肯定就是最小的
                } else mi[r] = min(mi[r], max(min(dp[au], mi[au] + c), mx1)); //这个可以不要
            }

            for (int i = 0; i < G[v].size(); i++) {
                int &au = G[v][i], &c = cost[v][i];
                in[au]--;
                if (used[au] == 0) {
                    dp[au] = max(dp[r] + c, dp[au]);
                    if (in[au] == 1)q.push(au);
                }
            }
        }
//        for (int i = 1; i <= n; i++) {
//            printf("[%d]= %d %d\n", i, dp[i], mi[i]);
//        }
        if (n == 1 || n == 2)printf("%d %d\n", 1, 0);
        else {
            dfs(r, -1, 0, 0, 0);
            printf("%d %d\n", ans1, ans2);
            for (int i = 1; i <= n; i++) {
                used[i] = 0;
                in[i] = 0;
                G[i].clear();
                cost[i].clear();
            }
        }
    }
#ifndef ONLINE_JUDGE
    cout << "运行时间:" << 1.0 * (clock() - prostart) / CLOCKS_PER_SEC << "s\n";
#endif // ONLIN_JUDGE
    return 0;
}
```