---
title: 2019 Multi-University Training Contest 5 1002 three arrays
date: 2019-08-09 22:34:01
tags:
    - 字典树
    - 杭电多校
categories:
    - ACM
    - 数据结构
mathjax: true

---
 [HDU 6625 three arrays](http://acm.hdu.edu.cn/showproblem.php?pid=6625)
**题意：** 给两个数组，求两个数组两两异或后最小字典序。
**题解：** 求字典序最小，也就是求值最小，如果是求一个数和另一个数组里面的一个值异或最小，很显然就是字典树，就是在字典树上优先取同位相同，没有再取同位相反。求两个数组异或之后字典序最小，其实也可以按照同样的方法求解。对两个数组分别做成一颗字典树，求两颗线段树异或之后字典序最小，就是两颗树异或值尽可能小。一开始我想到这个写法的时候，队友说如果`0 0`，和`1 1` 异或都等于`0` 先选哪一个，仔细思考一下，其实没有什么影响，统计一下两颗树当前节点下方，`0 1` 的个数，优先把 `0 0` `1 1` 匹配掉，然后再把`0 1` `1 0 `两种匹配掉。有点像在字典树上贪心.
复杂度两颗树最多有`n*31`个节点，最差就是每个树都跑一次，所以复杂度是$O(nlog(n))$
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
const int maxn = (int) 1e6 + 5;
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

int n;

inline int getpos(int x, int pos) {
    return ((x >> pos) & 1);
}

struct tree {
    int a[maxn * 10][3];
    int num[maxn * 10][3];
    int root = 0;
    int cnt = 1;

    void init() {
        cnt = 1;
    }

    void insert(int x) {
        int p = root, dep = 30;
        while (dep >= 0) {
            int nx = getpos(x, dep);
            if (num[p][nx] == 0) {
                a[p][nx] = cnt++;
                num[p][nx] = 1;
            } else {
                num[p][nx]++;
            }
            p = a[p][nx];
            dep--;
        }
    }
} t1, t2;

vector<int> v;

template<class T>
inline T min(T t1, T t2, T t3) {
    return min(t1, min(t2, t3));
}

void dfs(int now1, int now2, int num, int val, int dep) {
    if (dep == -1) {
        for (int i = 0; i < num; i++)
            v.emplace_back(val);
        return;
    }
    if (t1.num[now1][0] > 0 && t2.num[now2][0] > 0 && num > 0) {
        int ct = min(t2.num[now2][0], t1.num[now1][0], num);
        dfs(t1.a[now1][0], t2.a[now2][0], ct, val, dep - 1);
        t1.num[now1][0] -= ct;
        t2.num[now2][0] -= ct;
        num -= ct;
    }
    if (t1.num[now1][1] > 0 && t2.num[now2][1] > 0 && num > 0) {
        int ct = min(t1.num[now1][1], t2.num[now2][1], num);
        dfs(t1.a[now1][1], t2.a[now2][1], ct, val, dep - 1);
        t1.num[now1][1] -= ct;
        t2.num[now2][1] -= ct;
        num -= ct;
    }
    if (t1.num[now1][0] > 0 && t2.num[now2][1] > 0 && num > 0) {
        int ct = min(t2.num[now2][1], t1.num[now1][0], num);
        dfs(t1.a[now1][0], t2.a[now2][1], ct, val | (1 << dep), dep - 1);
        t1.num[now1][0] -= ct;
        t2.num[now2][1] -= ct;
        num -= ct;
    }
    if (t1.num[now1][1] > 0 && t2.num[now2][0] > 0 && num > 0) {
        int ct = min(t1.num[now1][1], t2.num[now2][0], num);
        dfs(t1.a[now1][1], t2.a[now2][0], ct, val | (1 << dep), dep - 1);
        t1.num[now1][1] -= ct;
        t2.num[now2][0] -= ct;
        num -= ct;
    }

}


int main() {
    f();
    int T;
    scanf("%d", &T);
    while (T--) {
        scanf("%d", &n);
        t1.init();
        t2.init();
        for (int i = 0; i < n; i++) {
            int a;
            scanf("%d", &a);
            t1.insert(a);
        }
        for (int j = 0; j < n; j++) {
            int a;
            scanf("%d", &a);
            t2.insert(a);
        }
        v.clear();
//        debug(t2.num[29][1]);
        dfs(0, 0, n, 0, 30);
        sort(v.begin(), v.end());
        for (int i = 0; i < n; i++) {
            printf("%d%c", v[i], i == n - 1 ? '\n' : ' ');
        }
    }
#ifndef ONLINE_JUDGE
    cout << "运行时间:" << 1.0 * (clock() - prostart) / CLOCKS_PER_SEC << endl;
#endif
    return 0;
}
```