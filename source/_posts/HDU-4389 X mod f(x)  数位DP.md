---
title: HDU-4389 X mod f(x)  数位DP
date: 2018-07-29 11:04:24
tags: CSDN迁移
---
  题目链接 ：[HDU - 4389 ](http://acm.hdu.edu.cn/showproblem.php?pid=4389)

 **X mod f(x)**

 **Time Limit: 4000/2000 MS (Java/Others) Memory Limit: 32768/32768 K (Java/Others)  
 Total Submission(s): 3619 Accepted Submission(s): 1409**  
  
 

 **Problem Description**

 Here is a function f(x):  
 int f ( int x ) {  
 if ( x == 0 ) return 0;  
 return f ( x / 10 ) + x % 10;  
 }

 Now, you want to know, in a given interval [A, B] (1 <= A <= B <= 109), how many integer x that mod f(x) equal to 0.

 

 

 **Input**

 The first line has an integer T (1 <= T <= 50), indicate the number of test cases.  
 Each test case has two integers A, B.

 

 

 **Output**

 For each test case, output only one line containing the case number and an integer indicated the number of x.

 

 

 **Sample Input**

 2

 1 10

 11 20

 

 

 **Sample Output**

 Case 1: 10

 Case 2: 3

 

 

 **Author**

 WHU

 

 

 **Source**

 [2012 Multi-University Training Contest 9](http://acm.hdu.edu.cn/search.php?field=problem&amp;key=2012+Multi-University+Training+Contest+9&amp;source=1&amp;searchmode=source)

 

 

 **Recommend**

 zhuyuanchen520

 

 数位DP

 所有数的和最大不超过82

 dp[pos][sum][mod][res] ，pos 第几位，sum 到第几位每个位数加起来的和，mod， 取模多少 。余数是res ,状态下有多少个数。  
 然后直接用for暴力取模的数的所有情况。

 套个数位DP的板子就行

 
```
 #include<iostream>
#include<cstdio>
#include<cstring>
using namespace std;
typedef int ll;
int dp[10][82][82][82];
// 所有数的和最大不超过82 dp[pos][sum][mod][res] ，pos 第几位，sum 到第几位每个位数加起来的和，mod， 取模多少 。余数是res ,状态下有多少个数。
int a[25];
ll dfs(int pos,ll sum,ll mod,ll res,bool limit) {   //状态 pos sum,mod,res, 上线情况limit
    if(sum>mod)return 0;                
    if(pos==-1) {                                   //当枚举完最后一位返回
        if(sum==mod&&res==0)return 1;               //满足条件返回1
        else return 0;                              //不满足返回0
    }
    if(limit==0&&dp[pos][sum][mod][res]!=-1)return dp[pos][sum][mod][res];//如果当前状态是已经有过记录且当前没有限制就直接返回已经记录的值
    ll up=limit?a[pos]:9,cnt=0;//最大可以枚举到up,如果当前没有上限就可以 0-9,否则只能到当前位的最大值，cnt 记录总共多少
    for(int i=0; i<=up; i++) {
        //跳转状态，前一位，总和加上值，取模数不变，更新余数，如果当前有上限，切加入的值已经到达当前上限 下一种情况才有上线  
        cnt+=dfs(pos-1,sum+i,mod,(res*10+i)%mod,limit&&i==a[pos]);
    }
    if(limit==0)dp[pos][sum][mod][res]=cnt; //当前状态是没有上限的情况下求的和，就可以记录当前状态
    return cnt;     
}
ll n,m;
ll solve(ll x) {
    int pos=0;
    while(x>0) {
        a[pos++]=x%10;
        x/=10;
    }
    ll ans=0;
    for(int i=1; i<82; i++) {
        ans+=dfs(pos-1,0,i,0,true);             //直接用for暴力取模的数的所有情况。
    }
    return ans;
}
int main() {
    ios_base::sync_with_stdio(0);
    int t;
    memset(dp,-1,sizeof(dp));
    cin>>t;
    int l=0;
    while(t--) {
        l++;
        cin>>n>>m;
        cout<<"Case "<<l<<": ";
        cout<<solve(m)-solve(n-1)<<endl;
    }
    return 0;
}
```
 

   
 