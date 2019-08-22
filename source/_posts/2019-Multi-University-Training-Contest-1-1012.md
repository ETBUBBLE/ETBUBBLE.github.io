---
title: 2019 Multi-University Training Contest 1 1012 Sequence
date: 2019-07-25 22:14:37
tags:
    - FFT/NTT/FWT
    - 杭电多校
categories:
    - ACM
    - 数论 
mathjax: true
---
[2019 Multi-University Training Contest 1](http://acm.hdu.edu.cn/search.php?field=problem&key=2019+Multi-University+Training+Contest+1&source=1&searchmode=source)
[HDU 6589 Sequence](http://acm.hdu.edu.cn/showproblem.php?pid=6589)
果然不看[大佬博客](https://www.cnblogs.com/Yinku/p/11232195.html)不会写题。顺手把板子也扒了。
**题解:** `x` 只有 `1 2 3 `三种情况。直接观察前缀.
可以发现当 `x=1` 的时候 $c_1$表示 `1`出现的个数
$$
 a[i]=\sum_{j=0}^{i}C_{c_1-1+j}^{j} * a[i-j]
$$
预理出 $C[j]=C_{c_1-1+j}^{j}$上式就变成了
$$
 a[i]=\sum_{j=0}^{i}C[j] * a[i-j]
$$
是不是绝对上述这个式子非常眼熟，就是卷积中的一项。。。
发现了这个，就是一个组合数加个NTT
当`x=2`的时候就把数组分奇数项和偶数项求一个上式
当`x=3`的时候就求`3`个
实际上，只要改一下$c[j]$
把按照`x=1`的求一下`x=2`
`c[j]=j%2==0>c[j/2]:0`
`x=3``c[j]=j%3==0?c[j/3]:0`
```c
#include "bits/stdc++.h"

using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
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


const LL mod = (LL) 998244353;
const int maxn = (int) 1e6 + 5;
const int INF = 0x7fffffff;
const LL inf = 0x3f3f3f3f;
const double eps = 1e-8;
const double PI = acos(-1);
#ifndef ONLINE_JUDGE
clock_t prostart = clock();
#endif

void f() {
#ifndef ONLINE_JUDGE
    freopen("../data.in", "r", stdin);
#endif
}

int t;
int n, m;
LL a[maxn];
int c[maxn];
LL b[maxn];

const int Comb_Maxn = 1e6 + 10;

LL Fac_inv[Comb_Maxn];
LL Fac[Comb_Maxn];

inline void Comb_init() {
    Fac_inv[0] = Fac[0] = 1;
    Fac_inv[1] = 1;
    for (int i = 1; i < Comb_Maxn; i++)
        Fac[i] = Fac[i - 1] * (LL) i % mod;
    for (int i = 2; i < Comb_Maxn; i++)
        Fac_inv[i] = (LL) (mod - mod / i) * Fac_inv[mod % i] % mod;
    for (int i = 1; i < Comb_Maxn; i++)
        Fac_inv[i] = Fac_inv[i - 1] * Fac_inv[i] % mod;
}

LL Comb(int n, int m) {
    if (m > n || m < 0 || n < 0)return 0;
    return Fac[n] * Fac_inv[m] % mod * Fac_inv[n - m] % mod;
}

typedef LL ll;
const int N = maxn;


struct NumberTheoreticTransform {
    int pow2(int x) {
        int res = 1;
        while (res < x)
            res <<= 1;
        return res;
    }


    inline LL pow_mod(ll x, int n) {
        ll res;
        for (res = 1; n; n >>= 1, x = x * x % mod)
            if (n & 1)
                res = res * x % mod;
        return res;
    }

    inline int add_mod(int x, int y) {
        x += y;
        return x >= mod ? x - mod : x;
    }

    inline int sub_mod(int x, int y) {
        x -= y;
        return x < 0 ? x + mod : x;
    }

    void NTT(LL a[], int n, int op) {
        for (int i = 1, j = n >> 1; i < n - 1; ++i) {
            if (i < j)
                swap(a[i], a[j]);
            int k = n >> 1;
            while (k <= j) {
                j -= k;
                k >>= 1;
            }
            j += k;
        }
        for (int len = 2; len <= n; len <<= 1) {
            LL g = pow_mod(3, (mod - 1) / len);
            for (int i = 0; i < n; i += len) {
                LL w = 1;
                for (int j = i; j < i + (len >> 1); ++j) {
                    LL u = a[j], t = 1ll * a[j + (len >> 1)] * w % mod;
                    a[j] = (u + t) % mod, a[j + (len >> 1)] = (u - t + mod) % mod;
                    w = 1ll * w * g % mod;
                }
            }
        }
        if (op == -1) {
            reverse(a + 1, a + n);
            LL inv = pow_mod(n, mod - 2);
            for (int i = 0; i < n; ++i)
                a[i] = 1ll * a[i] * inv % mod;
        }
    }

    void mul(LL A[], LL B[], int Asize, int Bsize) {
        int n = pow2(Asize + Bsize - 1);
        for (int i = Asize; i < n; ++i)
            A[i] = 0;
        for (int i = Bsize; i < n; ++i)
            B[i] = 0;
        NTT(A, n, 1);
        NTT(B, n, 1);
        for (int i = 0; i < n; ++i) {
            A[i] = 1ll * A[i] * B[i] % mod;
            B[i] = 0;
        }
        NTT(A, n, -1);
        return;
    }
} ntt;


template<typename T>
void read(T &w) {  //读入
    char c;
    while (!isdigit(c = getchar()));
    w = c & 15;
    while (isdigit(c = getchar()))w = w * 10 + (c & 15);
}

int main() {
    f();
    read(t);
    Comb_init();
    while (t--) {
        read(n);
        read(m);
        for (int i = 0; i < n; i++) {
            read(a[i]);
        }
        c[0] = c[1] = c[2] = c[3] = 0;
        while (m--) {
            int x;
            scanf("%d", &x);
            c[x]++;
        }
        if (c[1] > 0) {
            for (int i = 0; i < n; i++) {
                b[i] = Comb(c[1] + i - 1, i);
            }
            ntt.mul(a, b, n, n);
        }
        if (c[2] > 0) {
            for (int i = 0; i < n; i += 2) {
                b[i] += Comb(c[2] + i / 2 - 1, i / 2);
                b[i + 1] = 0;
            }
            ntt.mul(a, b, n, n);
        }
        if (c[3] > 0) {
            for (int i = 0; i < n; i += 3) {
                b[i] = Comb(c[3] + i / 3 - 1, i / 3);
            }
            ntt.mul(a, b, n, n);
        }
        LL ans = 0;
        for (int i = 0; i < n; i++) {
            ans ^= (i + 1LL) * a[i];
        }
        printf("%lld\n", ans);

    }
#ifndef ONLINE_JUDGE
    cout << "运行时间:" << 1.0 * (clock() - prostart) / CLOCKS_PER_SEC << "s\n";
#endif // ONLIN_JUDGE
    return 0;
}
```