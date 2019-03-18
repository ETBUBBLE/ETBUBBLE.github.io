---
title: 2013-2014 Summer Petrozavodsk Camp, Andrew Stankevich Contest 44 (ASC 44)
date: 2018-08-31 20:35:51
tags: CSDN迁移
---
  暑训最后一场组队训练赛，特么故意的，把别人的WF练习题给我们写，写了半天才签到两题。靠！

 
## [B - Braess's Paradox](https://vjudge.net/problem/Gym-100518B)

 [Gym - 100518B ](https://vjudge.net/problem/156405/origin)

 题意：有几个点，每个点到下一个点之间有两条路。上面一条路的通过时间是 A*K1+B,下面一条通过时间是C*K2+D，要两条路通过的时间数尽量相同，K1+K2=1(K1,K2是经过的人流量占总人数的比例)。然后中间的点可以建驿站，如果中间的点不建驿站，就相当于直接从起点到终点，只有两条路，如果建了驿站，就相当于从起点到驿站，再从驿站到终点。

 题解：前两个直接算出来就行，一个驿站都不建，就相当于两条路，把所有点上面那条路，ai.bi,加起来就是上面那一条路的A,B，同理，下面一条路就是所有的,ci,di加起来。每个都建就是相当于一个个点走过去，暴力啊，一个点到另一个点的时间，然后全加起来就行了。后面两个就是求最小通过时间的和最大通过时间，看似很难，其实就是一个很简单的DP，N^2的复杂度不会超时。首先预处理从起点到当前点的A,B,C,D。然后每个点的时间就是从前面某一个点建驿站的最小值，最大值转移过来。直接过来上面路的A，B就用两个前缀相减。

 
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
typedef long long ll;
typedef unsigned long long ull;
typedef pair<ll,int> P;

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
const int maxn=5e3+5;
const int INF=0x7fffffff;
const int inf=0x3f3f3f3f;
const double eps=1e-8;
int n;
int a[maxn],b[maxn],c[maxn],d[maxn];
double qza[maxn],qzb[maxn],qzc[maxn],qzd[maxn];
double dp1[maxn],dp2[maxn];
double ans1,ans2,ans3,ans4;
int main() {
    freopen("braess.in","r",stdin);
    freopen("braess.out","w",stdout);
    scanf("%d",&n);
    for(int i=1; i<=n; i++) {
        scanf("%d%d%d%d",&a[i],&b[i],&c[i],&d[i]);
    }
    double A=0,B=0,C=0,D=0;
    for(int i=1; i<=n; i++) {       //第一种结果都不建，A相当于所有点之间的a[i]相加
        A+=a[i];
        B+=b[i];
        C+=c[i];
        D+=d[i];
    }
    double k;
    if(A+C!=0) {                    
        k=min(1.0,(D+C-B)/(A+C));//k1最大值不能超过1 
        ans1=k*A+B;
        if(k<=0) {              //k1最小值不能小于0，如果小于等于0说明所有人都走下面一条路
            ans1=C+D;
        }
    } else {
        ans1=min(B,D);          //如果AC都等于零，那就直接判断B，D大小
    }
    ans2=0;
    for(int i=1; i<=n; i++) {
        A=a[i];                 //所有的点一个个算
        B=b[i];
        C=c[i];
        D=d[i];
        double temp;
        if(A+C!=0) {
            k=min(1.0,(D+C-B)/(A+C));
            temp=k*A+B;
            if(k<=0) {
                temp=C+D;
            }
        } else {
            temp=min(B,D);
        }
        ans2+=temp;
    }
    for(int i=1; i<=n; i++) {           //求前缀
            qza[i]=qza[i-1]+a[i];
            qzb[i]=qzb[i-1]+b[i];       
            qzc[i]=qzc[i-1]+c[i];
            qzd[i]=qzd[i-1]+d[i];
        dp1[i]=inf;
    }
    dp1[0]=0;
    for(int i=1; i<=n; i++) {
        for(int j=0; j<i; j++) {
            A=qza[i]-qza[j];
            B=qzb[i]-qzb[j];            //ABCD等于上一个状态和当前状态的差值，
            C=qzc[i]-qzc[j];
            D=qzd[i]-qzd[j];
            double temp;
            if(A+C!=0) {
                k=min(1.0,(D+C-B)/(A+C));
                temp=k*A+B;
                if(k<=0) {
                    temp=C+D;
                }
            } else {
                temp=min(B,D);
            }
            dp1[i]=min(dp1[i],dp1[j]+temp);     //dp1保留最小值，从j驿站转移到i驿站的最小值
            dp2[i]=max(dp2[i],dp2[j]+temp);     //最大值
        }
    }
    ans3=dp1[n];
    ans4=dp2[n];
    printf("%.10f\n%.10f\n%.10f\n%.10f\n",ans1,ans2,ans3,ans4);

    return 0;
}
```
 
## [I - Intelligent Tourist](https://vjudge.net/problem/Gym-100518I)

 [Gym - 100518I ](https://vjudge.net/problem/156411/origin)

 题意：有N场考试，第i考试需要复习pi天，考试时间是di，考试的时间没法复习，如果没有复习多天那场考试就不会去，就会去复习其他考试，中间有些天有活动，活动时间是s-t活动时间不会去复习，问最多能通过机场考试。

 题解：贪心，这题贼他妈傻逼，漏看了一个条件，DEBUG一个小时。。。，从后面往前面扫，每次选需要复习时间最少的一场考试，因为这个时间只能给后面的所以不用担心选的考试时间已经过了，至于考试时间不能复习，你直接把考试也当作复习的时间，不去考试就相当于少复习这种科目一天，去了就相当于多复习一天。。。

 
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
typedef long long ll;
typedef unsigned long long ull;
typedef pair<ll,int> P;

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
const int maxn=1e5+5;
const int INF=0x7fffffff;
const int inf=0x3f3f3f3f;
const double eps=1e-8;
int n,m;
struct one {
    ll d,p;
    int id;
    bool operator<(const one a)const {
        return d<a.d;
    }
} X;
struct two {
    ll st,en;
    bool operator<(const two a)const {
        return st<a.st;
    }
} H;
priority_queue<one> q1;
priority_queue<two> q2;
priority_queue<P,vector<P>,greater<P> >q;

int main() {
    freopen("intelligent.in","r",stdin);
    freopen("intelligent.out","w",stdout);
    while(~scanf("%d",&n)&&n) {
        vector<int> v;
        for(int i=0; i<n; i++) {
            scanf("%I64d%I64d",&X.d,&X.p);
            X.p++;
            X.id=i+1;
            q1.push(X);
        }
        scanf("%d",&m);
        for(int i=0; i<m; i++) {
            scanf("%I64d%I64d",&H.st,&H.en);
            q2.push(H);
        }

        while(q2.size()) {
            if(q2.top().st>q1.top().d) {
                q2.pop();
            } else break;
        }
        while(q1.size()) {
            X=q1.top();
            q1.pop();
            ll temp;
            if(q1.size()==0)temp=0;
            else temp=q1.top().d;
            ll tim=0;
            tim=X.d-temp;
            while(q2.size()) {
                H=q2.top();
                if(H.st>temp) {
                    q2.pop();
                    tim-=H.en-H.st+1;
                } else break;
            }

            if(X.p==0) {
                v.push_back(X.id);
            } else q.push(P(X.p,X.id));

            while(tim>0&&q.size()) {
                P temp2=q.top();
                q.pop();
                if(tim>=temp2.first) {
                    tim-=temp2.first;
                    v.push_back(temp2.second);
                } else {
                    temp2.first-=tim;
                    tim=0;
                    q.push(temp2);
                }
            }
        }
        while(q1.size())q1.pop();
        while(q2.size())q2.pop();
        while(q.size())q.pop();
        sort(v.begin(),v.end());
        int ans=v.size();
        printf("%d\n",ans);
        for(int i=0; i<ans; i++) {
            printf("%d",v[i]);
            if(ans==i+1)printf("\n");
            else printf(" ");
        }
        if(ans==0)puts("");
    }
    return 0;
}
```
 

   
 