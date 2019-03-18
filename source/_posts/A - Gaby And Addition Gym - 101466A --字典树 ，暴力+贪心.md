---
title: A - Gaby And Addition Gym - 101466A --字典树 ，暴力+贪心
date: 2018-08-19 21:03:17
tags: CSDN迁移
---
  题目链接 ：[http://codeforces.com/gym/101466/problem/A](http://codeforces.com/gym/101466/problem/A)

 A. Gaby And Addition

 time limit per test

 6.0 s

 memory limit per test

 1024 MB

 input

 standard input

 output

 standard output

 Gaby is a little baby who loves playing with numbers. Recently she has learned how to add 2 numbers using the standard addition algorithm which we summarize in 3 steps:

 
  1. Line up the numbers vertically matching digits places. 
  3. Add together the numbers that share the same place value from right to left. 
  5. Carry if necessary. it means when adding two numbers we will get something like this:

 ![](http://codeforces.com/predownloaded/c1/cc/c1cc3569a5348992a24eeb2984de8e883b782f5c.png)

 Unfortunately as Gaby is too young she doesn't know what the third step means so she just omitted this step using her own standard algorithm (Gaby's addition algorithm). When adding two numbers without carrying when necessary she gets something like the following:

 ![](http://codeforces.com/predownloaded/25/b0/25b04ceb9ae1868ae3b6b5771434f9ef98655dd0.png)

 Gaby loves playing with numbers so she wants to practice the algorithm she has just learned (in the way she learned it) with a list of numbers adding every possible pair looking for the pair which generates the largest value and the smallest one.

 She needs to check if she is doing it correctly so she asks for your help to find the largest and the smallest value generated from the list of numbers using Gaby's addition algorithm.

 Input

 The input starts with an integer _n_ (2 ≤ _n_ ≤ 106) indicating the number of integers Gaby will be playing with. The next line contains _n_numbers _n__i_ (0 ≤ _n__i_ ≤ 1018) separated by a single space.

 Output

 Output the smallest and the largest number you can get from adding two numbers from the list using Gaby's addition algorithm.

 Examples

 input

 Copy

 
```
 6
17 5 11 0 42 99

```
 output

 Copy

 
```
 0 99

```
 input

 Copy

 
```
 7
506823119072235413 991096248449924896 204242310783332529 778958050378192979 384042493592684633 942496553147499866 410043616343857825

```
 output

 Copy

 
```
 52990443860776502 972190360051424498

```
 Note

 In the first sample input this is how you get the minimum and the maximum value

 ![](http://codeforces.com/predownloaded/ca/b7/cab79490fb5f04f783ae8580168aad02a4c0b8f7.png)

 

 题意：给 n个数求不进位加法，两个数和的最大值，最小值。

 题解：分别对每个数字拆分成 18个位，每个位是 0-9 的数字，然后用每个位建一个字典树。

 就形成了一棵以0结点为根节点，然后每层分配0-9 儿子节点的字典树，然后每次查询和当前值相加最大值和最小值，分别每次从取模10最大的和最小的节点匹配。

 例如 当前位是 5 最大值直接从当前位儿子节点 4开始找 如果存在直接求两个数的和，否则继续3 2 1.。。。

 
```
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

using namespace std;
typedef long long ll;
typedef unsigned long long ull;
typedef pair<int,int> P;

#define bug printf("*********\n");
#define debug(x) cout<<"["<<x<<"]" <<endl;
#define mid (l+r)/2
#define chl 2*k+1
#define chr 2*k+2
#define lson l,mid,chl
#define rson mid,r,chr
#define pb push_back
#define mem(a,b) memset(a,b,sizeof(a));

const long long mod=1e9+7;
const int maxn=1e6+5;
const int INF=0x7fffffff;
const int inf=0x3f3f3f3f;
const double eps=1e-8;
int n;
int cnt=1;
struct Trie {
    int son[10];
    void init() {
        for(int i=0; i<10; i++) {
            son[i]=-1;
        }
    }
} tree[maxn*20];
ll p[20];
void insert(int r,int pos,ll val) {
    if(pos==-1)return ;
    ll v=val/p[pos]%10;
    if(tree[r].son[v]==-1) {
//        printf("%d",v);
        tree[cnt].init();
        tree[r].son[v]=cnt++;
    }
    insert(tree[r].son[v],pos-1,val);
}

ll findmx(int r,int pos,ll val) {
    if(pos==-1)return 0;
    ll v=val/p[pos]%10;
    for(int i=9-v; i>=0; i--) {
        if(tree[r].son[i]!=-1) {
            return (p[pos]*((v+i)%10))+findmx(tree[r].son[i],pos-1,val);
        }
    }
    for(int i=9; i>9-v; i--)
        if(tree[r].son[i]!=-1)
            return (p[pos]*((v+i)%10))+findmx(tree[r].son[i],pos-1,val);
}
ll findmi(int r,int pos,ll val) {
    if(pos==-1)return 0;
    int v=val/p[pos]%10;
    for(int i=10-v; i<=9; i++)
        if(tree[r].son[i]!=-1) {
            return (p[pos]*((v+i)%10))+findmi(tree[r].son[i],pos-1,val);
        }
    for(int i=0; i<10-v; i++)
        if(tree[r].son[i]!=-1)
            return (p[pos]*((v+i)%10))+findmi(tree[r].son[i],pos-1,val);
}
int main() {
    scanf("%d",&n);
    p[0]=1;
    for(int i=1; i<=18; i++) {
        p[i]=p[i-1]*10;
    }
    tree[0].init();
    ll mx=-1e18,mi=1e18;
    for(int i=0; i<n; i++) {
        ll x;
        scanf("%lld",&x);
        if(i!=0) {
            mi=min(mi,findmi(0,18,x));
            mx=max(mx,findmx(0,18,x));
        }
        insert(0,18,x);
//        puts("");
    }
    printf("%lld %lld\n",mi,mx);
    return 0;
}

```
 

   
 