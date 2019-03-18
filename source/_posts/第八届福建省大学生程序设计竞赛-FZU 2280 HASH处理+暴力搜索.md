---
title: 第八届福建省大学生程序设计竞赛-FZU 2280 HASH处理+暴力搜索
date: 2018-08-21 18:37:03
tags: CSDN迁移
---
  **题目：[Problem 2280 Magic](http://acm.fzu.edu.cn/problem.php?pid=2280)**

 
## ![](http://acm.fzu.edu.cn/image/prodesc.gif) Problem Description

 Kim is a magician, he can use n kinds of magic, number from 1 to n. We use string Si to describe magic i. Magic Si will make Wi points of damage. Note that Wi may change over time.

 Kim obey the following rules to use magic:

 Each turn, he picks out one magic, suppose that is magic Sk, then Kim will use all the magic i satisfying the following condition:

 1. Wi<=Wk

 2. Sk is a suffix of Si.

 Now Kim wondering how many magic will he use each turn.

 

 Note that all the strings are considered as a suffix of itself.

 
## ![](http://acm.fzu.edu.cn/image/prodesc.gif) Input

 First line the number of test case T. (T<=6)

 For each case, first line an integer n (1<=n<=1000) stand for the number of magic.

 Next n lines, each line a string Si (Length of Si<=1000) and an integer Wi (1<=Wi<=1000), stand for magic i and it’s damage Wi.

 Next line an integer Q (1<=Q<=80000), stand for there are Q operations. There are two kinds of operation.

 “1 x y” means Wx is changed to y.

 “2 x” means Kim has picked out magic x, and you should tell him how many magic he will use in this turn.

 Note that different Si can be the same.

 
## ![](http://acm.fzu.edu.cn/image/prodesc.gif) Output

 For each query, output the answer.

 
## ![](http://acm.fzu.edu.cn/image/prodesc.gif) Sample Input

 1 5 abracadabra 2 adbra 1 bra 3 abr 3 br 2 5 2 3 2 5 1 2 5 2 3 2 2

 
## ![](http://acm.fzu.edu.cn/image/prodesc.gif) Sample Output

 3 1 2 1

 
## ![](http://acm.fzu.edu.cn/image/prodesc.gif) Source

 第八届福建省大学生程序设计竞赛-重现赛（感谢承办方厦门理工学院）

 题目：给你n个字符串以及权值，两种操作 一种 更新字符串对应的权值 ，查询 输出所有以当前字符串为后缀且对应权值小于当前字符串权值的个数。

 题解：首先hash 预处理所有能供以当前字符串为后缀的字符串，直接n^2暴力就行。。。

 然后查询直接暴力搜索小于当前字符串权值的。

 数据只有1000 的范围随便暴力啊。。。。

 
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
const int maxn=1e3+5;
const int INF=0x7fffffff;
const int inf=0x3f3f3f3f;
const double eps=1e-8;
const int seed=131;
ull Hash[maxn][maxn];
ull po[maxn];
char ch[maxn][maxn];
int t,n;
int len[maxn];
bool mp[maxn][maxn];
int val[maxn];
void init() {
    mem(mp,0);
    po[0]=1;
    for(int i=1; i<1002; i++) {
        po[i]=po[i-1]*seed;
    }
    for(int i=1; i<=n; i++) {
        for(int j=1; j<=len[i]; j++) {
            Hash[i][j]=Hash[i][j-1]*seed+ch[i][j];
        }
    }
    for(int i=1; i<=n; i++) {
        for(int j=1; j<=n; j++) {
            if(len[j]<len[i])continue;
            else {
                int l=len[j]-len[i];
                if(Hash[j][len[j]]-Hash[j][l]*po[len[i]]==Hash[i][len[i]]) {
                    mp[i][j]=1;
                }
            }
        }
    }
}

void read(int &sum) {
    sum=0;
    int flag=0;
    char ch=getchar();
    while(!(ch>='0'&&ch<='9')) {
        if (ch == '-') {
            flag = 1;
        }
        ch=getchar();
    }
    while(ch>='0'&&ch<='9')sum=sum*10+ch-48,ch=getchar();
    if(flag)sum*=-1;
}

int main() {
    read(t);
    while(t--) {
        read(n);
        for(int i=1; i<=n; i++) {
            scanf("%s%d",ch[i]+1,&val[i]);
            len[i]=strlen(ch[i]+1);
        }
        init();
        int q;
        read(q);
        while(q--) {
            int op;
            read(op);
            int x,y;
            if(op==1) {
                read(x);
                read(y);
                val[x]=y;
            } else {
                read(x);
                int ans=0;
                for(int i=1;i<=n;i++)if(mp[x][i]&&val[x]>=val[i])ans++;
                printf("%d\n",ans);
            }
        }
    }
    return 0;
}

```
 

   
 