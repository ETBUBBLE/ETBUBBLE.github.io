---
title: Codeforces Round 526 (Div. 2)
date: 2018-12-13 19:32:36
tags: CSDN迁移
---
  很久没写代码了。随便刷了一下CF

 
## [C. The Fair Nut and String](http://codeforces.com/contest/1084/problem/C)

 先统计一下被'b'分隔的‘a’有多少个，放到一个数组里面，比如说，ababaaba a[0]=1,a[1]=1,a[2]=2,a[3]=1;

 然后算一下总方案数，这个有点难解释，就是取这个之前所有的方案数*这个里面的个数，再加上只取这个里面的个数。

 差不多就是这么算的:sum[i]=sum[i-1]*a[i]+a[i];

 
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
#include<bitset>

using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
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

char s[maxn];
LL ans[maxn];
long long n,k,num,sum,ct;
int main() {
    scanf("%s",s);
    int l = strlen(s);
    k=0;
    ct=1;
    for(int i = 0 ; i < l ; i ++) {
        if(s[i]=='a'){
            n++;
            ct=0;
        }
        if(s[i]=='b'){
            if(ct==0)ans[k++]=n;
            ct=1;
            n=0;
        }
    }
    if(!ct)ans[k++]=n;
    
    for(int i = 0 ; i < k ; i++){
        sum+=sum*ans[i];
        sum%=mod;
        sum+=ans[i];
        sum%=mod;
    }
    cout<<sum<<endl;
    return 0;
}
```
 
# [D. The Fair Nut and the Best Path](http://codeforces.com/contest/1084/problem/D)

 这题是个树形DP，每次选最边上的节点，如果一个节点周围的节点只有一个没走过了就把这个节点加到队列里面取判断。

 如果你选的节点是最大的油量中的一个，那么，一定等于周围两个最大且大于零的节点想加，且加上自己的油量。具体看代码吧，然后dp每次保存到这个节点最大油量。

 
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
#include<bitset>

using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
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
int n,in[maxn];
LL w[maxn],ans,dp[maxn];
int used[maxn];
struct two {
    LL to,c;
};
int k;
vector<two> G[maxn];
int main() {
    scanf("%d",&n);
    for(int i = 1 ; i <= n ; i ++) {
        scanf("%lld",&w[i]);
    }
    two t;
    for(int i = 0 ; i < n-1; i++) {
        int u,v,c;
        scanf("%d%d%d",&u,&v,&c);
        t.c=c;
        t.to=v;
        G[u].push_back(t);
        t.to=u;
        G[v].push_back(t);
        in[u]++;
        in[v]++;
    }
    queue<int>q;
    for(int i = 1 ; i <= n ; i++) {
        if(in[i]<=1) {
            q.push(i);
        }
    }
    LL max1=0,max2=0;
    LL ans=0;
    while(q.size()) {
        int node=q.front();
        q.pop();
        max1=max2=0;
        for(auto i:G[node]) {
            if(used[i.to]==1) {
                LL temp=dp[i.to]-i.c;
                if(temp>max1) {
                    swap(max1,temp);
                }
                if(temp>max2) {
                    swap(max2,temp);
                }
            }
            else {
                in[i.to]--;
                if(in[i.to]<=1){
                    q.push(i.to);
                }
            }
        }
        used[node]=1;
        LL temp=max1+max2+w[node];
        ans=max(ans,temp);
        dp[node]=temp-max2;
    }

    cout<<ans<<endl;
    return 0;
}

```
 

   
 