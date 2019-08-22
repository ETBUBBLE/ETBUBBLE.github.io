---
title: Codeforces Round 573 Div 2
date: 2019-07-13 10:27:17
tags:
    - codeforces
categories:
    - ACM
    - 比赛
---
## [A - Tokitsukaze and Enhancement](https://codeforces.com/contest/1191/problem/A)
简单题不与说明
```c++
#include<bits/stdc++.h>
 
using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
typedef pair<int,int> P;
 
#define VNAME(value) (#value)
#define bug printf("*********\n");
#define debug(x) cout<<"["<<VNAME(x)<<" = "<<x<<"]"<<endl;
#define mid (l+r)/2
#define chl 2*k+1
#define chr 2*k+2
#define lson l,mid,chl
#define rson mid+1,r,chr
#define pb push_back
#define mem(a,b) memset(a,b,sizeof(a));
 
const long long mod=1e9+7;
const int maxn=1e6+5;
const int INF=0x7fffffff;
const LL inf=0x3f3f3f3f;
const double eps=1e-8;
void f() {
#ifndef ONLINE_JUDGE
    freopen("dat.in", "r", stdin);
#endif // ONLIN_JUDGE
}
 
int main() {
    f();
    int x;
    cin>>x;
    x=x%4;
    if(x==0) {
        printf("1 A\n");
    } else if(x==1) {
        printf("0 A\n");
    } else if(x==2) {
        printf("1 B\n");
    } else  printf("2 A\n");
    return 0;
}
```

## [B - Tokitsukaze and Mahjong](https://codeforces.com/contest/1191/problem/B)
```c++
#include<bits/stdc++.h>
 
using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
typedef pair<int,int> P;
 
#define VNAME(value) (#value)
#define bug printf("*********\n");
#define debug(x) cout<<"["<<VNAME(x)<<" = "<<x<<"]"<<endl;
#define mid (l+r)/2
#define chl 2*k+1
#define chr 2*k+2
#define lson l,mid,chl
#define rson mid+1,r,chr
#define pb push_back
#define mem(a,b) memset(a,b,sizeof(a));
 
const long long mod=1e9+7;
const int maxn=1e6+5;
const int INF=0x7fffffff;
const LL inf=0x3f3f3f3f;
const double eps=1e-8;
void f() {
#ifndef ONLINE_JUDGE
    freopen("dat.in", "r", stdin);
#endif // ONLIN_JUDGE
}
char s[20];
vector<int >v[3],v2,v3;
 
int f(int x) {
    sort(v[x].begin(),v[x].end());
    if(v[x].size()==0)return 3;
    if(v[x].size()==1)return 2;
    else if(v[x].size()==2) {
        if(v[x][1]==v[x][0])return 1;
        if(v[x][1]==v[x][0]+1)return 1;
        if(v[x][1]==v[x][0]+2)return 1;
        return 2;
    } else {
        if(v[x][1]==v[x][0]&&v[x][1]==v[x][2])return 0;
        if(v[x][1]==v[x][0]+1&&v[x][1]==v[x][2]-1)return 0;
        if(v[x][1]==v[x][0]||v[x][1]==v[x][2])return 1;
        if(v[x][1]==v[x][0]+1||v[x][1]==v[x][0]+2||v[x][1]==v[x][2]-1||v[x][1]==v[x][2]-2)return 1;
        return 2;
    }
}
int main() {
    int ans=2;
    for(int i=0; i<3; i++) {
        scanf("%s",s);
        if(s[1]=='s') {
            v[0].push_back(s[0]);
        } else if(s[1]=='p') {
            v[1].push_back(s[0]);
        } else v[2].push_back(s[0]);
    }
    for(int i=0; i<3; i++) {
//        debug(i);
        ans=min(ans,f(i));
    }
    cout<<ans<<endl;
    return 0;
}
```
## [C - Tokitsukaze and Discard Items](https://codeforces.com/contest/1191/problem/C)

```c++
#include<bits/stdc++.h>
 
using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
typedef pair<int,int> P;
 
#define VNAME(value) (#value)
#define bug printf("*********\n");
#define debug(x) cout<<"["<<VNAME(x)<<" = "<<x<<"]"<<endl;
#define mid (l+r)/2
#define chl 2*k+1
#define chr 2*k+2
#define lson l,mid,chl
#define rson mid+1,r,chr
#define pb push_back
#define mem(a,b) memset(a,b,sizeof(a));
 
const long long mod=1e9+7;
const int maxn=1e6+5;
const int INF=0x7fffffff;
const LL inf=0x3f3f3f3f;
const double eps=1e-8;
void f() {
#ifndef ONLINE_JUDGE
    freopen("dat.in", "r", stdin);
#endif // ONLIN_JUDGE
}
 
LL n,m,k;
LL a[maxn];
int main() {
    cin>>n>>m>>k;
    for(int i=0; i<m; i++) {
        scanf("%lld",&a[i]);
    }
    sort(a,a+m);
    LL sum=0,ans=0,num=0;
    LL page=1;
    for(int i=0; i<m; i++) {
        if(a[i]<=page*k+sum) {
            num++;
        } else {
            if(num==0) {
                page=(a[i]-sum+k-1)/k;
                num++;
            } else {
                ans++;
                sum+=num;
                page=(a[i]-sum+k-1)/k;
                num=1;
            }
        }
    }
    if(num!=0)ans++;
    printf("%lld\n",ans);
    return 0;
}
```

## [D - Tokitsukaze, CSL and Stone Game](https://codeforces.com/contest/1191/problem/D)

首先这题是简单粗暴，因为选择到两个相同的就输了，说明每一个都不相同，最终状态肯定是 0 1 2 3 .... n-1
这种状态肯定是必输，无法动弹。所以最终都会变成这个状态，判断一下到这个状态的奇偶就是答案。
另外还有一开始就输了的状态，比如  3 4 4 两个一样的，只能选一样的，但是选了有一个和他相同，还有 0 0 一开始就有两个0 还有就是 5 5 5 三个一样的或者两对两个一样的，这四种状态绝对是直接输了。

```c++
By ET_BUBBLE, contest:
Codeforces Round #573 (Div. 2), problem: (D) Tokitsukaze, CSL and Stone Game, Accepted, #
#include<bits/stdc++.h>

using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
typedef pair<int,int> P;

#define VNAME(value) (#value)
#define bug printf("*********\n");
#define debug(x) cout<<"["<<VNAME(x)<<" = "<<x<<"]"<<endl;
#define mid (l+r)/2
#define chl 2*k+1
#define chr 2*k+2
#define lson l,mid,chl
#define rson mid+1,r,chr
#define pb push_back
#define mem(a,b) memset(a,b,sizeof(a));

const long long mod=1e9+7;
const int maxn=1e6+5;
const int INF=0x7fffffff;
const LL inf=0x3f3f3f3f;
const double eps=1e-8;
void f() {
#ifndef ONLINE_JUDGE
    freopen("dat.in", "r", stdin);
#endif // ONLIN_JUDGE
}

LL n;
LL a[maxn];
int flag=1,num;
map<LL,LL> mp;
LL sum=0;
int main() {
    cin>>n;
    for(int i=0; i<n; i++) {
        cin>>a[i];
    }
    sort(a,a+n);
    for(int i=0; i<n; i++) {
        if(mp.find(a[i])!=mp.end()) {
            num++;
            if(mp.find(a[i]-1)!=mp.end())flag=0;
        }
        mp[a[i]]++;

    }
    if(num>1||mp[0]>=2||flag==0) {
        puts("cslnb");
        return 0;
    }
    if(n==1) {
        puts((a[0]&1)?"sjfnb":"cslnb");
    } else {
        int flag=1;
        sum=0;
        for(int i=0; i<n; i++) {
            if(a[i]<i) {
                flag=0;
            } else {
                sum+=a[i]-i;
            }
        }
        if(flag==0) {
            puts("cslnb");
            return 0;
        }
        puts(sum%2==1?"sjfnb":"cslnb");
    }
    return 0;
}

```
## [F - Tokitsukaze and Strange Rectangle](https://codeforces.com/contest/1191/problem/F)

题意：自己读去
题解：先按照,y从大到小排序在按照x从小到大排序，然后每次判断一层y。
![](https://i.loli.net/2019/07/13/5d2949979136b55250.png)
先判断第一层 1 2 6 7 10   ~~（假设）y=10~~
然后判断第二层 4 8      ~~(假设) y = 9~~
第一层能够出现的不同的选法是  5\*(5-1)/2;
第二层会受到第一层的影响 1 2 在 4 前面，所以
要选 4 的矩形情况 是
![](https://i.loli.net/2019/07/13/5d294a89c6f3152134.png)
红色l到右边蓝色r的所有矩形，会选上4,同理选上8又不和前面的重复就只能是这样了。
![](https://i.loli.net/2019/07/13/5d294be332ff329273.png)
然后又可以发现，如果有第3层 ，前面两层对第3层的影响只与x的出现有关，每次判断一层只需要考虑上面出现的 x
的影响。
先离散化一下，然后用树状数组求一下这个点前面有多个点，后面有多少个点，然后乘一下就可以了。
```c++
#include<bits/stdc++.h>

using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
typedef pair<int,int> P;

#define VNAME(value) (#value)
#define bug printf("*********\n");
#define debug(x) cout<<"["<<VNAME(x)<<" = "<<x<<"]"<<endl;
#define mid (l+r)/2
#define chl 2*k+1
#define chr 2*k+2
#define lson l,mid,chl
#define rson mid+1,r,chr
#define pb push_back
#define mem(a,b) memset(a,b,sizeof(a));

const long long mod=1e9+7;
const int maxn=1e6+5;
const int INF=0x7fffffff;
const LL inf=0x3f3f3f3f;
const double eps=1e-8;
void f() {
#ifndef ONLINE_JUDGE
    freopen("dat.in", "r", stdin);
#endif // ONLIN_JUDGE
}

int bit[maxn+1],pos;
int sum(int i) {
    int s=0;
    while(i>0) {
        s +=bit[i];
        i-=i&-i;
    }
    return s;
}
void add(int i,int x) {
    while(i<=pos) {
        bit[i]+=x;
        i+=i&-i;
    }
}


int n;
struct node {
    int x,y;
} p[maxn];

bool cmp(node &o1,node &o2) {
    if(o1.y==o2.y)return o1.x<o2.x;
    return o1.y>o2.y;
}

LL ans=0;
unordered_map<int,int>mp;
int a[maxn];
int main() {
    f();
    scanf("%d",&n);
    for(int i=0; i<n; i++) {
        scanf("%d%d",&p[i].x,&p[i].y);
        a[i]=(p[i].x);
    }
    pos=1;
    sort(a,a+n);
    for(int i=0; i<n; i++) {
        if(i==0) {
            mp[a[0]]=pos++;
        } else if(a[i]!=a[i-1]) {
            mp[a[i]]=pos++;
        }
    }
    for(int i=0; i<n; i++) {
        p[i].x=mp[p[i].x];
    }
    sort(p,p+n,cmp);

    int len=0;
    a[len++]=p[0].x;
    add(p[0].x,1);
    int mx=pos;
    for(int i=1; i<n; i++) {
        if(p[i].y==p[i-1].y) {
            a[len++]=p[i].x;
            if(sum(p[i].x)-sum(p[i].x-1)==0)add(p[i].x,1);
        } else {
            int la=0;
            //            sort(a,a+len);
            for(int j=0; j<len; j++) {
                int i=a[j];
                ans+=1LL*(sum(i)-sum(la))*(sum(mx)-sum(i-1));
                la=i;
            }
            len=0;
            a[len++]=(p[i].x);
            if(sum(p[i].x)-sum(p[i].x-1)==0)add(p[i].x,1);
        }
    }
    int la=0;
    //    sort(a,a+len);
    for(int j=0; j<len; j++) {
        int i=a[j];
        ans+=1LL*(sum(i)-sum(la))*(sum(mx)-sum(i-1));
        la=i;
    }
    printf("%lld\n",ans);
    return 0;
}
```
