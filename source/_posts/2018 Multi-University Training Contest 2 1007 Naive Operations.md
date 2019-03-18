---
title: 2018 Multi-University Training Contest 2 #1007 Naive Operations
date: 2018-07-29 11:33:51
tags: CSDN迁移
---
  [HDU 6135](http://acm.hdu.edu.cn/showproblem.php?pid=6315)

 **Naive Operations**

 **Time Limit: 6000/3000 MS (Java/Others) Memory Limit: 502768/502768 K (Java/Others)  
 Total Submission(s): 2438 Accepted Submission(s): 1074**  
  
 

 **Problem Description**

 In a galaxy far, far away, there are two integer sequence a and b of length n.  
 b is a static permutation of 1 to n. Initially a is filled with zeroes.  
 There are two kind of operations:  
 1. add l r: add one for al,al+1...ar  
2. query l r: query ∑ri=l⌊ai/bi⌋

 

 

 **Input**

 There are multiple test cases, please read till the end of input file.  
 For each test case, in the first line, two integers n,q, representing the length of a,b and the number of queries.  
 In the second line, n integers separated by spaces, representing permutation b.  
 In the following q lines, each line is either in the form 'add l r' or 'query l r', representing an operation.  
1≤n,q≤100000, 1≤l≤r≤n, there're no more than 5 test cases.

 

 

 **Output**

 Output the answer for each 'query', each one line.

 

 

 **Sample Input**

 5 12

 1 5 2 4 3

 add 1 4

 query 1 4

 add 2 5

 query 2 5

 add 3 5

 query 1 5

 add 2 4

 query 1 4

 add 2 5

 query 2 5

 add 2 2

 query 1 5

 

 

 **Sample Output**

 1

 1

 2

 4

 4

 6

 

 **Source**

 [2018 Multi-University Training Contest 2](http://acm.hdu.edu.cn/search.php?field=problem&amp;key=2018+Multi-University+Training+Contest+2&amp;source=1&amp;searchmode=source)

 

 比赛的时候写了半天没写出来，结果发现是线段树板子敲错了-_-|||

 给一段区间，区间的值全部加+1 

 查询 区间 a[i]/b[i]向下取整的和。

 因为查询a[i]/b[i]向下取整，直接求有点难。

 所以我们换个操作，我们每次区间加一，变成把每个值减一，每次减到0的时候ai/bi的值就会+1，我们记录这个+1，再把值重新更新为bi，查询的时候查询+1 的总和。

 用线段树保留最小值，当出现最小值为0的时候把cnt++，值更新为b[r]，因为每次只会加+1所以总数不会太大

 如： 1 2 3 4 5

 add 1 4

 区间值 0 1 2 3 5 ,出现最小值 0 所以区间 cnt++ 把零变为 b[i]

 1 1 2 3 5

 然后一直下去，查询直接查询cnt 总和就行。

 
```
 #include<iostream>
#include<algorithm>
#include<cstring>
#include<string>
#include<vector>
#include<cstdio>
#include<cmath>
#include<queue>
#include<map>
#include<set>
#include<stack>

using namespace std;
typedef long long ll;
typedef unsigned long long ull;

const long long mod=1e9+7;
const int maxn=1e5+25;
const int INF=0x7fffffff;
const int inf=0x3f3f3f3f;
const double eps=1e-8;
int b[4*maxn]; 

int n,q;
int dat[4*maxn],lazy[4*maxn];//lazy保存区间所加的值 dat 为最小值。
int res;
int cnt[maxn*4];            //   保存每个区间里面的总个数

void init(int l,int r,int k) {   //初始化
    int chl=2*k+1,chr=2*k+2,mid=(l+r)>>1;
    if(r-l==1) {
        lazy[k]=cnt[k]=0;
        dat[k]=b[r];    
        return ;
    } else {
        lazy[k]=cnt[k]=0;
        init(l,mid,chl);
        init(mid,r,chr);
        dat[k]=min(dat[chl],dat[chr]);
    }
}

int sum(int a,int c,int l,int r,int k) {//查询总个数
    int chl=2*k+1,chr=2*k+2,m=(l+r)/2;
    if(c<=l||a>=r) {                    //不在区间内
        return 0;
    } else if(a<=l&&r<=c) {             //覆盖这个区间
        return cnt[k];
    } else {
        lazy[chl]+=lazy[k];             //把lazy 更新下去
        lazy[chr]+=lazy[k];
        lazy[k]=0;
        dat[k]=min(dat[chl]+lazy[chl],dat[chr]+lazy[chr]);
        return sum(a,c,l,(l+r)/2,k*2+1)+sum(a,c,(l+r)/2,r,k*2+2);
    }
}
void update(int a,int c,int l,int r,int k) {
    int chl=2*k+1,chr=2*k+2,mid=(l+r)/2;
    if(c<=l||a>=r) {
        return ;
    } else if(a<=l&&r<=c) {             
        if(lazy[k]+dat[k]-1<=0) {       //如果所覆盖的区间减一出现值小于等于0 就去找那个值
            if(r-l==1) {
                cnt[k]++;               //找到后 cnt ++
                dat[k]=b[r];
                lazy[k]=0;              //把当前结点的值重新更新为b[r]
                return ;
            }
            lazy[chl]+=lazy[k];         //向下更新lazy
            lazy[chr]+=lazy[k];
            lazy[k]=0;
            update(a,c,l,mid,chl);      //向左右儿子结点找
            update(a,c,mid,r,chr);      
            if(r-l!=1) {
                cnt[k]=cnt[chl]+cnt[chr];
                dat[k]=min(dat[chl]+lazy[chl],dat[chr]+lazy[chr]);  //更新值
            }
            return;
        }
        lazy[k]--;              //如果没有就直接把lazy减一
    } else {                    // 大区间有一部分在小区间内
        lazy[chl]+=lazy[k];     
        lazy[chr]+=lazy[k];
        update(a,c,l,mid,chl);
        update(a,c,mid,r,chr);      
        lazy[k]=0;
        dat[k]=min(dat[chl]+lazy[chl],dat[chr]+lazy[chr]);      //更新
        if(r-l!=1)cnt[k]=cnt[chl]+cnt[chr];
    }
}
char ch[10];
int l,r;
int main() {
    while(scanf("%d%d",&n,&q)!=EOF) {
        for(int i=1; i<=n; i++) {
            scanf("%d",&b[i]);
        }
        init(0,n,0);
        while(q--) {
            scanf("%s%d%d",ch,&l,&r);
            if(ch[0]=='a') {
                update(l-1,r,0,n,0);            //我写的线段是是(l,r] 所以要记得-1
            } else {
                printf("%d\n",sum(l-1,r,0,n,0));   
            }
        }
    }
    return 0;
}
```
 

   
 